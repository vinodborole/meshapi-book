---
type: Web Page
title: Caching | Mesh API Docs
description: Cut cost and latency with the gateway response cache and provider-side
  prompt caching — how each one works, when it applies, and how to opt out.
resource: https://developers.meshapi.ai/docs/guides/caching
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Caching

MeshAPI has **two independent caching layers**. They solve different problems and can be used together:

The gateway cache is on by default and needs no code changes. Provider prompt caching is opt-in per request via `cache_control` markers.

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

Anything not in that list does **not** affect the cache key. Two things follow that surprise people:

- **`stream` is not part of the key.** A streaming request can be served from a response originally stored by a non-streaming one; MeshAPI replays it as normal SSE chunks. Your streaming client sees no difference.
- **`user` is not part of the key.** Passing a different`user` value does not give you a different cache entry.

### When caching applies

A request is eligible only if **all** of these hold:

The temperature rule is the one that catches people out. If you send `"temperature": 0.7`, you get **no caching at all**, no matter how many times you repeat the identical prompt. Deterministic workloads — classification, extraction, structured output — should send `temperature: 0` (or omit it) to benefit.

### Detecting a cache hit

A response served from the cache carries:

There is no `X-Cache: MISS` header. The header is **present only on a hit** — treat its absence as a miss rather than checking for a `MISS` value.

In your usage records, a cached request is logged with `cache_source: "gateway"`, so you can measure hit rate and savings from the [Usage API](/docs/guides/usage-monitoring-api).

### Turning it off

Caching is enabled by default. You can opt out per request in two ways:

###### Request header

###### Request body

Both skip the cache read *and* the cache write for that request.

`"cache": true` does **not** force caching on. The flag is deliberately asymmetric: `false` is an unconditional opt-out, but `true` only means “cache if otherwise permitted” — it still respects your key’s settings and every eligibility rule above. There is no way to cache a `temperature: 0.9` request.

To disable caching for every request on a key, set it on the key itself rather than per request — see the [Account Configuration Checklist](/docs/guides/account-configuration-checklist).

### Cache isolation

By default each account gets its **own** cache namespace: your responses are never served to another tenant, even for an identical prompt. Accounts may opt into a shared namespace, where entries are keyed by content hash alone and hit rates are higher because the cache is warmed by all participants. Shared caching is off unless you ask for it.

### Freshness

Entries live for **24 hours**. There is no explicit invalidation API — if you need a guaranteed-fresh answer, send `X-Mesh-Cache: no-store`.

## Provider prompt caching

Provider prompt caching is a different mechanism: instead of reusing a whole response, the provider stores the processed **prefix** of your prompt and charges less for it on subsequent requests. It pays off when you send a large, stable block — a long system prompt, a document, a few-shot set — followed by a small varying question.

Unlike the gateway cache, this one still calls the provider and still generates a fresh completion. It reduces the cost of the *input* tokens, not the call.

### Marking a prefix

Add a `cache_control` marker to the content you want cached. MeshAPI forwards it to providers that support server-side prompt caching.

Each marker defines a breakpoint that caches **everything before it**. Put markers at the end of the stable region, immediately before the part that changes between requests.

### Where markers are honoured

Whether `cache_control` reaches the provider depends on **which provider serves the model**, not on the model name alone. The same model can behave differently depending on how it is routed.

OpenAI’s direct API performs prompt caching **automatically** on long prompts — you do not mark it, and you cannot control it from here. Markers are removed on that route not because caching is unavailable, but because the provider manages it itself.

An unsupported marker is **silently removed** — it never errors. Your request succeeds and returns a normal completion, and nothing in the response indicates the marker was dropped. Never assume a marker took effect: confirm it against `cached_tokens` in the response usage block.

On Bedrock a request may carry at most **4** cache breakpoints. If you send more, MeshAPI keeps the last 4 — later breakpoints cover longer prefixes, so keeping them preserves the most caching value.

You can see which provider served any request in your [usage records](/docs/guides/usage-monitoring-api).

### Measuring the benefit

Cached input tokens are reported as `cached_tokens` in the usage block of the response and in your usage records, where the request is logged with `cache_source: "provider"`. Compare `cached_tokens` against `prompt_tokens` to see what fraction of your prompt was served from the provider’s cache.

## Choosing between them

- **Repeating identical requests** (dashboards, evals, retries, deterministic pipelines) → the gateway cache does this for free, automatically. Just keep`temperature` at`0` .
- **Varying questions over a large fixed context** (document Q&A, long system prompts, few-shot) → the gateway cache never hits, because the messages differ every time. Use provider prompt caching.
- **Both** are valid together: a repeated request hits the gateway cache first and never reaches the provider at all.

## Related

- [Memory](/docs/guides/memory) — why memory-attached requests bypass the response cache
- [Usage & Monitoring API](/docs/guides/usage-monitoring-api) — measuring hit rate and savings
- [Pricing](/docs/introduction/pricing) — how cached tokens are billed

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/caching
