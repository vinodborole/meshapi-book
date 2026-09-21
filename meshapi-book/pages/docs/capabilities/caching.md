---
type: Web Page
title: Caching - Mesh API
description: Cut cost and latency with the gateway response cache and provider-side
  prompt caching — how each one works, what it costs, and when it pays off.
resource: https://developers.meshapi.ai/docs/capabilities/caching
timestamp: '2026-09-21T12:28:28.555532+00:00'
---

**two independent caching layers**. They solve different problems and can be used together:

The gateway cache is on by default and needs no code changes. Provider prompt caching is opt-in per request via 

`cache_control` markers.
## Gateway response cache

When a request is byte-for-byte equivalent to one served recently, MeshAPI returns the stored response and never calls the provider. The result is a response in single-digit milliseconds at no token cost.
### What counts as “identical”

The cache key is a SHA-256 hash over exactly these fields:
- `model`
- the resolved upstream `provider`
- `messages`
- `max_tokens`
- `temperature`
- `stop`
- `response_format`
- `service_tier` , when the request selected one — see[Service Tiers](/docs/capabilities/service-tiers)

**not**affect the cache key. Two things follow that surprise people:

- **`stream` is not part of the key.** A streaming request can be served from a response originally stored by a non-streaming one; MeshAPI replays it as normal SSE chunks. Your streaming client sees no difference.
- **`user` is not part of the key.** Passing a different`user` value does not give you a different cache entry.

### When caching applies

A request is eligible only if
**all**of these hold:

### Detecting a cache hit

A response served from the cache carries:
There is no 

`X-Cache: MISS` header. The header is **present only on a hit**— treat its absence as a miss rather than checking for a`MISS` value.`X-Cache: HIT` responses to measure your hit rate. MeshAPI records the hit internally too, but that marker is not returned by the [Usage API](/docs/reference/usage-api), so it cannot be filtered on from outside.

### Turning it off

Caching is enabled by default. You can opt out per request in two ways:
- Request header
- Request body

*and*the cache write for that request. To disable caching for every request on a key, set it on the key itself rather than per request — see the

[Account Configuration Checklist](/docs/getting-started/account-checklist).

### Cache isolation

By default each account gets its
**own**cache namespace: your responses are never served to another tenant, even for an identical prompt. Accounts may opt into a shared namespace, where entries are keyed by content hash alone and hit rates are higher because the cache is warmed by all participants. Shared caching is off unless you ask for it.

### Freshness

Entries live for
**24 hours**. There is no explicit invalidation API — if you need a guaranteed-fresh answer, send

`X-Mesh-Cache: no-store`.
## Provider prompt caching

Provider prompt caching is a different mechanism: instead of reusing a whole response, the provider stores the processed
**prefix**of your prompt and charges less for it on subsequent requests. It pays off when you send a large, stable block — a long system prompt, a document, a few-shot set — followed by a small varying question. Unlike the gateway cache, this one still calls the provider and still generates a fresh completion. It reduces the cost of the

*input*tokens, not the call.

### Marking a prefix

Add a`cache_control` marker to the content you want cached. MeshAPI forwards it to providers that support server-side prompt caching.
**everything before it**. Put markers at the end of the stable region, immediately before the part that changes between requests.

### Where markers are honoured

Whether`cache_control` reaches the provider depends on **which provider serves the model**, not on the model name alone. The same model can behave differently depending on how it is routed.

OpenAI’s direct API performs prompt caching 

**automatically**on long prompts — you do not mark it, and you cannot control it from here. Markers are removed on that route not because caching is unavailable, but because the provider manages it itself.
**4**cache breakpoints. If you send more, MeshAPI keeps the last 4 — later breakpoints cover longer prefixes, so keeping them preserves the most caching value. You can see which provider served any request in your

[usage records](/docs/reference/usage-api).

### Reading the result

Every response tells you exactly what the cache did, under`usage.prompt_tokens_details`:
`prompt_tokens` **includes**both numbers, so the tokens processed fresh at the normal rate are

`prompt_tokens - cached_tokens - cache_write_tokens`.
Read these defensively: `prompt_tokens_details` is absent entirely when nothing was cached, and `cache_write_1h_tokens` appears only when a 1h write actually happened. Use `.get(...)` rather than indexing.
On a streaming request the usage block arrives on the 

**final chunk**. You do not need to ask for it — MeshAPI requests usage from the provider on every stream, so the cache breakdown is there either way.
### What it costs

Cached tokens are not billed at the normal input rate. Every
**Claude**model on MeshAPI uses the same two multipliers:

Measured end to end on 

`anthropic/claude-sonnet-5` with a 6,800-token system block, same prompt each time:
So a hit costs about an eighth of the uncached call, and the write premium is repaid inside the first hit — two calls sharing a prefix are already ~31% cheaper than two uncached ones.

**What decides whether it pays is reuse, not prompt share.**A segment costs 1.25× once and 0.10× on every later read, against 1.00× each time uncached — so it is ahead the first time it is read (1.25 + 0.10 < 2.00), however small a fraction of the prompt it is. A 2,000-token prefix on a 50,000-token request still pays, provided it is reused.

### Holding a prefix for an hour

The default entry expires after ~5 minutes. If your calls are further apart than that, add a TTL and the prefix survives an hour instead:
**2× input**instead of 1.25× — so it only pays when it converts writes into reads. Rule of thumb: if the gap between calls sharing a prefix is reliably under 5 minutes, stay on the default; if it’s typically longer, the 1h write is cheaper than paying a 5m write every time.

Writes made under a 1h TTL are reported separately as 

`cache_write_1h_tokens` in `prompt_tokens_details`, so you can tell the two apart on the bill.
On Bedrock the TTL applies to 

**Claude**models only, and it is all-or-nothing per request: MeshAPI reports the 1h rate only when*every*breakpoint in the request carries`ttl: "1h"`. Mix 5m and 1h markers in one call and the whole request bills at the 5m rate. A model whose price row has no 1h rate configured also bills 1h writes at the 5m rate.
### Multi-turn: move the breakpoint forward

The biggest win is a conversation, not a single call. Each turn resends everything before it, so with one marker pinning the stable head and a second rolling onto the newest message, every turn reads the whole previous turn and writes only the delta.
**0.14×**the uncached cost while the conversation grew.

### When a marker doesn’t take effect

A marker that does nothing is silent — the request succeeds and returns a normal completion. If`prompt_tokens_details` comes back empty, work down this list:
## The cached block is too small

The cached block is too small

Providers refuse to store a prefix below a minimum, and it is model-dependent: 

**1,024 tokens**on Sonnet and Opus,**2,048**on Haiku. Under it you get no cache entry and no error. MeshAPI does not validate this for you.
## Your tool list changed

Your tool list changed

Tool definitions sit 

**ahead of**the system block in the cached prefix, so editing, adding or removing a tool invalidates everything after it. Measured: the same system block with one extra tool schema returned`cached_tokens: 0` and re-wrote all 7,153 tokens. Keep `tools` byte-stable across the calls meant to share a prefix — including the order of the array.
## More than five minutes passed

More than five minutes passed

A default 

`ephemeral` entry lives about **5 minutes**, refreshed each time it is read. A loop that idles longer between turns pays the write again and again and never collects the discount — see[Holding a prefix for an hour](#holding-a-prefix-for-an-hour).
## You trimmed the conversation from the front

You trimmed the conversation from the front

Dropping the oldest messages to stay under a limit changes the 

*start*of the prompt, which is exactly what the cache keys on — so nothing after it can be reused. Summarize into the system block instead of dropping the head — otherwise every trim turns that turn’s write into one that is never read.
## The route doesn't honour markers

The route doesn't honour markers

See 

[Where markers are honoured](#where-markers-are-honoured)— direct OpenAI, Together, DeepInfra, Qwen and Vertex strip the field, and Bedrock applies it only to Claude and Nova. Check which provider actually served the request in your[usage records](/docs/reference/usage-api).
A request that fails upstream does 

**not**warm the cache, so a rejected call costs you nothing in cache writes — the next valid call is a normal cold write.
## Choosing between them

- **Repeating identical requests** (dashboards, evals, retries, deterministic pipelines) → the gateway cache does this for free, automatically. Just keep`temperature` at`0` .
- **Varying questions over a large fixed context** (document Q&A, long system prompts, few-shot, agent loops) → the gateway cache never hits, because the messages differ every time. Use provider prompt caching: the prefix is reused on every question, which is exactly when it pays.
- **Prompts with nothing stable in them** (each call unique, or calls further apart than the TTL) → neither. A marker here is written and never read, which costs 25% more than not caching.
- **Both** are valid together: a repeated request hits the gateway cache first and never reaches the provider at all.

## Related

- [Memory](/docs/capabilities/memory) — why memory-attached requests bypass the response cache
- [Usage & Monitoring API](/docs/reference/usage-api) — measuring hit rate and savings
- [Pricing](/docs/getting-started/pricing) — how cached tokens are billed

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/caching
