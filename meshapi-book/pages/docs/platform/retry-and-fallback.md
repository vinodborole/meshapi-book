---
type: Web Page
title: Retry & Fallback - Mesh API
description: How Mesh API automatically retries and reroutes requests when an upstream
  provider returns a transient error — which status codes trigger it, and how same-provider
  retries, cross-provider fallback, and model fallback fit together.
resource: https://developers.meshapi.ai/docs/platform/retry-and-fallback
timestamp: '2026-09-14T12:21:17.301704+00:00'
---

`503`, an overloaded region, a brief rate-limit
spike. Mesh API absorbs these transient failures for you: instead of returning
the first error it sees, the gateway automatically **retries**and, if needed,

**reroutes**your request before responding. This is built into the platform and happens

**server-side**. You don’t enable it, configure it, or change your code — you send a normal request and Mesh API does the resilience work behind the scenes.

## Two independent layers

Alongside the gateway’s built-in behaviour, you can add resilience in the SDK. The two operate at different scopes and are safe to combine:
A typical setup: let the 

**gateway**absorb provider-level blips for a model transparently, with no code change, and use the

**SDK fallback chain**to switch to an entirely different model if the primary is degraded everywhere. The SDK never retries a stream; the gateway protects one only until its first token is sent, as described below. See

[SDK Resilience](/sdk/resilience).

**Every other endpoint runs this policy.**Chat completions,

`/v1/responses`,
embeddings, and image, video and audio generation all resolve your key’s
routing policy, retry, plan cross-provider and cross-model fallback targets,
and share the same circuit breaker.One consequence is worth knowing before it appears on an invoice: because
cross-
**model**fallback applies to these endpoints too, an image, video or embeddings request can be served by a different model than the one you asked for, and is billed at the rate of the model that actually served it. It is not silent — the response carries

`X-Mesh-Routing-Model` naming the substitute, and
the response body’s own `model` field always reports what served the request.
## What happens when a request fails

When an upstream returns an error, the gateway responds in escalating steps. It only moves to the next step if the previous one couldn’t recover:
1

Retry the same provider

The gateway waits briefly, then re-sends the request to the same provider.
Transient blips often clear on the second or third try.

2

Fall back to the same model on a different provider

If the provider keeps failing, the gateway re-issues the request for the

**exact same model**through a different upstream that serves it. You still get the model you asked for.
3

Fall back to a different model

If the requested model can’t be served anywhere right now, the gateway can
route to a comparable alternate model so the request still succeeds.

[response headers](#seeing-what-happened)or your dashboard logs.

## Which errors trigger retry and fallback

Only
**transient**failures — errors that could plausibly succeed on a second attempt — trigger retries and fallback. By default these are the HTTP status codes:

The same set governs both same-provider retries and cross-provider fallback: a
provider must return one of these (or time out with no response at all) for the
gateway to try again.

### Errors that are never retried

Some failures mean something is wrong with the request itself — trying again would only produce the same error. These are
**terminal**: the gateway returns them immediately, with no retry and no fallback.

A 

`429` (rate limit) is treated as transient and *is*retried, because it typically clears on its own or resolves once the request is routed to another provider.
### Capability mismatches fall over, even though they aren’t transient

One case sits outside the status table above. When a model simply cannot do what you asked — tool calling inside a stream, vision input, a feature it doesn’t implement — the provider reports it as a`400` or `422`, which is not a transient
status.
Retrying that same model is futile; the answer will never change. But the
**next-ranked model may support the feature**. So a capability mismatch is treated as terminal

*for that model*while still being eligible for fallover to a different one.

This is why a request asking for an unsupported feature can still succeed on a
model you didn’t name. If no alternate can serve it either, you get a clean
error — a capability mismatch is never silently swallowed.

## How retries are paced

Retries against the same provider use
**exponential backoff with jitter**— the gateway waits a short, randomized delay that grows with each attempt, so a provider recovering from a spike isn’t immediately hammered again. A provider is retried only a small number of times before the gateway moves on to fallback. The whole sequence — the initial call, same-provider retries, and every fallback hop — shares

**one attempt budget and one wall-clock deadline**. Retries and fallbacks stop once that budget is spent, so a struggling upstream can never make your request hang indefinitely: you get a result, or a final error, within a bounded window. Because the budget is

*shared*, the primary target could otherwise consume all of it and leave nothing for a fallback hop. The gateway holds one attempt back while alternates remain, so a failing primary cannot starve the fallback it exists to trigger. The practical consequence: a request is

**not**retried indefinitely across every provider that serves your model — it is a budget, not a promise.

## Per-key routing policy

Beyond the platform defaults, a key can carry an explicit
**routing policy**. It is per-key opt-in — a key has no policy until you set one from the

[Dashboard](https://app.meshapi.ai)(

**API Keys → edit a key → Resilience**). Once set, the gateway applies it to that key’s non-streaming chat completions with no client changes. The policy is a JSON object with three blocks —

`retry`, `fallback`, and `budget`:
- **`retry`** — how the gateway retries the*same* target before giving up or falling back.
- **`fallback`** — when`cross_provider_same_model` is`true` , the gateway re-issues the request for the**same model against a different provider** .`models` optionally constrains the candidate set.
- **`budget`** — a hard stop across all attempts, so retries and fallbacks can never run unbounded.

### Ceilings

Values above these limits are clamped when you save a policy:
### What each parameter controls

### Worked example

Policy:`retry.max_retries = 1`, `fallback.enabled` and `cross_provider_same_model`
both `true`, `budget.max_attempts = 4`. The primary provider is having an outage:
1. **Attempt 1** — primary provider returns`503` . Transient, in`retry_on_status` , budget remains → retry.
2. **Attempt 2** — after`base_ms` backoff, primary again returns`503` .`max_retries` (1) is exhausted for this target → check fallback: eligible.
3. **Attempt 3** — second provider, same model →`200 OK` .

`X-Mesh-Routing-Attempts: 3` and
`X-Mesh-Routing-Fallback: true`, and the dashboard log row shows a **Provider fallback**badge and

*Retried ×2*. Had attempt 1 failed with a

`401` instead
(terminal), it would have failed immediately — attempts 2 and 3 never happen,
whatever the policy says.
## Failing providers are taken out of rotation

Alongside per-request retries, Mesh API runs a platform-wide
**circuit breaker**. When a specific model-and-provider combination starts failing repeatedly, it is temporarily pulled from the routing pool for everyone, then re-checked automatically after a short cooldown. This keeps requests from being routed into an upstream that is already known to be down, so fallback skips straight to a healthy provider. This is a global safety mechanism managed by the platform — it isn’t tied to any individual request or key.

## What retry and fallback cost you

**Failed attempts are not billed.**Only an attempt that actually returned a response is charged, so the retries and fallback hops described above are free. If the gateway tries a provider three times and then reroutes, you pay once — for the answer you received, not for the attempts it took to get there. This is worth stating plainly because the intuitive assumption is the opposite: resilience does not mean paying for resilience. When the gateway falls back to the

**same model on a different provider**, the model that answers is the one you requested, and billing is unchanged. When it falls back to a

**different model**, you are billed for the model that actually served the request — which may be more or less than the model you asked for. The response tells you what happened, so the substitution is never silent.

### Charges that surprise people

Three cases where a bill is larger than a naive “one request, one charge” reading predicts. None of them is a retry.
**Automatic model selection runs a small internal model to pick the target, and that call is billed alongside the answer. It is a fraction of the cost of the request it routes, but it is not free — see**

`model: "auto"` bills a classifier call.
[Auto Routing](/docs/capabilities/auto-routing).

**A different model means a different rate.**Covered above, and repeated here because it is the one that shows up as an unexpected line on an invoice: model fallback protects availability, not price.

## Seeing what happened

For
**non-streaming**chat completions, the gateway reports what it did through response headers:

Which specific upstream served the request is internal and isn’t reported — the
headers tell you 

*that*a retry or fallback happened and how many attempts it took. Your Mesh API dashboard logs show the same information per request, including for streamed calls that don’t carry these headers.

### Not to be confused with `X-BYOK-Fallback-Triggered`

The `X-Mesh-Routing-*` headers describe **routing**fallback. That is a different mechanism from

[BYOK](/docs/capabilities/byok)

**credential**fallback:

Both can appear on the same response — a request can fall back from your BYOK
credentials 

*and*be routed across providers — but they answer different questions. In the dashboard Logs they render as distinct

**BYOK fallback**and

**Provider fallback**badges.

### Turning credential fallback off: `allow_fallback`

BYOK credential fallback is controlled per credential by **, which is**

`allow_fallback`
**enabled by default**. When your own provider key fails with an auth or rate-limit error, the gateway retries the request on Mesh API’s shared platform credentials so it still succeeds.

**Why you might deliberately disable it.**Some organisations must guarantee their traffic only ever touches

*their own*provider account — for contractual reasons, data-residency commitments, or because their agreement with the upstream provider governs how that data may be processed. Silently completing a request on Mesh API’s shared credentials would break that guarantee, even though it would make the individual request succeed.Set

`allow_fallback` to `false` on the credential and a failure of your key is
returned to you as an error instead of being rerouted. You are choosing a failed
request over an unexpected one — which, under an isolation commitment, is the
correct trade.
### Seeing it from the SDK

The Mesh API SDKs parse these headers into a structured`gateway-routing` event,
so you can log exactly what the gateway did alongside your own client-side
retries and fallbacks. See [SDK Resilience](/sdk/resilience).

# Citations

1. Source page: https://developers.meshapi.ai/docs/platform/retry-and-fallback
