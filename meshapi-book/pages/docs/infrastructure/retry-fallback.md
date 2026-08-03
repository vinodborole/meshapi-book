---
type: Web Page
title: Retry & Fallback | Mesh API Docs
description: How Mesh API automatically retries and reroutes requests when an upstream
  provider returns a transient error — which status codes trigger it, and how same-provider
  retries, cross-provider fallback, and model fallback fit together.
resource: https://developers.meshapi.ai/docs/infrastructure/retry-fallback
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Retry & Fallback

Retry & Fallback

# Retry & Fallback

Upstream providers occasionally fail for reasons that have nothing to do with
your request — a momentary `503`, an overloaded region, a brief rate-limit
spike. Mesh API absorbs these transient failures for you: instead of returning
the first error it sees, the gateway automatically **retries** and, if needed,
**reroutes** your request before responding.

This is built into the platform and happens **server-side**. You don’t enable
it, configure it, or change your code — you send a normal request and Mesh API
does the resilience work behind the scenes.

## What happens when a request fails

When an upstream returns an error, the gateway responds in escalating steps. It only moves to the next step if the previous one couldn’t recover:

### Retry the same provider

The gateway waits briefly, then re-sends the request to the same provider. Transient blips often clear on the second or third try.

All of this runs inside a single request. From your side, you make one call and
receive one response — the retries and reroutes are invisible unless you look
at the [response headers](/docs/infrastructure/retry-fallback#seeing-what-happened) or your dashboard logs.

## Which errors trigger retry and fallback

Only **transient** failures — errors that could plausibly succeed on a second
attempt — trigger retries and fallback. By default these are the HTTP status
codes:

The same set governs both same-provider retries and cross-provider fallback: a provider must return one of these (or time out with no response at all) for the gateway to try again.

### Errors that are never retried

Some failures mean something is wrong with the request itself — trying again
would only produce the same error. These are **terminal**: the gateway returns
them immediately, with no retry and no fallback.

A `429` (rate limit) is treated as transient and *is* retried, because it
typically clears on its own or resolves once the request is routed to another
provider.

## How retries are paced

Retries against the same provider use **exponential backoff with jitter** — the
gateway waits a short, randomized delay that grows with each attempt, so a
provider recovering from a spike isn’t immediately hammered again. A provider is
retried only a small number of times before the gateway moves on to fallback.

The whole sequence runs inside an overall **attempt and time budget**. Retries
and fallbacks stop once that budget is reached, so a struggling upstream can
never make your request hang indefinitely — you get a result (or a final error)
within a bounded window.

## Failing providers are taken out of rotation

Alongside per-request retries, Mesh API runs a platform-wide **circuit
breaker**. When a specific model-and-provider combination starts failing
repeatedly, it is temporarily pulled from the routing pool for everyone, then
re-checked automatically after a short cooldown. This keeps requests from being
routed into an upstream that is already known to be down, so fallback skips
straight to a healthy provider.

This is a global safety mechanism managed by the platform — it isn’t tied to any individual request or key.

## A note on model fallback and billing

When the gateway falls back to the **same model on a different provider**, the
model that answers is the one you requested, and billing is unchanged.

When it falls back to a **different model**, you are billed for the model that
actually served the request. In both cases the response tells you what happened
so there are no surprises.

**Streaming requests** are protected only *before the first token is sent*.
Once a stream has started emitting output, an error mid-stream is returned
as-is — a partially streamed response can’t be retried or rerouted without
duplicating output.

## Seeing what happened

For **non-streaming** chat completions, the gateway reports what it did through
response headers:

Which specific upstream served the request is internal and isn’t reported — the
headers tell you *that* a retry or fallback happened and how many attempts it
took. Your Mesh API dashboard logs show the same information per request,
including for streamed calls that don’t carry these headers.

Don’t confuse `X-Mesh-Routing-Fallback` with `X-BYOK-Fallback-Triggered`.
Routing fallback is about **providers and models** (this page). The
[BYOK](/byok) header is about **credentials** — your own provider key failing
and falling back to Mesh API’s shared platform credentials. They’re separate
mechanisms and can both appear on the same response.

The specific status codes, retry counts, timing, and fallback behaviour described here are the platform’s current defaults and may change in the future as we tune reliability. Treat retry and fallback as a best-effort resilience layer rather than a fixed contract, and always handle a final error response in your own code.

# Citations

1. Source page: https://developers.meshapi.ai/docs/infrastructure/retry-fallback
