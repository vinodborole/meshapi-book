---
type: Web Page
title: Retry & Fallback - Mesh API
description: How Mesh API automatically retries and reroutes requests when an upstream
  provider returns a transient error — which status codes trigger it, and how same-provider
  retries, cross-provider fallback, and model fallback fit together.
resource: https://developers.meshapi.ai/docs/platform/retry-and-fallback
timestamp: '2026-08-10T07:50:31.317333+00:00'
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

**SDK fallback chain**to switch to an entirely different model if the primary is degraded everywhere. Neither layer retries streaming responses. See

[SDK Resilience](/sdk/resilience).

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
## How retries are paced

Retries against the same provider use
**exponential backoff with jitter**— the gateway waits a short, randomized delay that grows with each attempt, so a provider recovering from a spike isn’t immediately hammered again. A provider is retried only a small number of times before the gateway moves on to fallback. The whole sequence runs inside an overall

**attempt and time budget**. Retries and fallbacks stop once that budget is reached, so a struggling upstream can never make your request hang indefinitely — you get a result (or a final error) within a bounded window.

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

## A note on model fallback and billing

When the gateway falls back to the
**same model on a different provider**, the model that answers is the one you requested, and billing is unchanged. When it falls back to a

**different model**, you are billed for the model that actually served the request. In both cases the response tells you what happened so there are no surprises.

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

### Seeing it from the SDK

The Mesh API SDKs parse these headers into a structured`gateway-routing` event,
so you can log exactly what the gateway did alongside your own client-side
retries and fallbacks. See [SDK Resilience](/sdk/resilience).

# Citations

1. Source page: https://developers.meshapi.ai/docs/platform/retry-and-fallback
