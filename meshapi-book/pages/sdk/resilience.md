---
type: Web Page
title: Resilience - Mesh API
description: Configurable retries, a client-side model fallback chain, and observability
  for retries and fallbacks.
resource: https://developers.meshapi.ai/sdk/resilience
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

- **Transport retry** — automatic retries on transient HTTP failures, with a configurable policy.
- **Model fallback chain** — try the next model when the primary fails.
- **`debug` / `logger`** — see exactly which requests were retried and which were served by a fallback (client- and gateway-side).

[Resilient Routing](/docs/platform/retry-and-fallback). The two layers are independent and safe to combine — here is how a single

`create()` call flows through both:
Read it bottom-up for the guarantees: a terminal error (auth, validation, billing) short-circuits every layer; a transient one is first absorbed by gateway-side retries/fallback (no client involvement), then by SDK transport retries of the same request, and only then does the SDK switch models.
## Transport retry

Every
**non-streaming**request retries on

`429` / `502` / `503` / `504` with exponential backoff + jitter, honouring `Retry-After` (default: 3 retries, 500 ms base, 30 s max). **Streams never retry.**The policy is configurable:

- Python
- Node.js
- Go
- Java

`maxRetries` / `max_retries` option still works and maps onto `retry.maxRetries`; an explicit `retry` value wins when both are set.
## Model fallback chain

Non-streaming chat completions can fall back to other
**models**when the primary fails with a transient error (default

`502` / `503` / `504`, after transport retries are exhausted). Configure a chain client-wide, or override it per call:
- Python
- Node.js
- Go
- Java

The client-side 

`fallbackModels` chain is **distinct**from the`models` request parameter. `models` is a server-side, provider-handled ordered list sent in the request body; `fallbackModels` is a client-side directive the SDK acts on locally and never puts on the wire.
## Seeing what happened: `debug` and `logger`

Set `debug` to print a readable line to stderr on every retry and fallback:
`logger` — it receives every `retry`, `fallback`, and `gateway-routing` event:
- Python
- Node.js
- Go
- Java

### `gateway-routing` events

A `gateway-routing` event reports the **server-side**resilience the gateway itself performed for your request — the per-key

`routing_policy`’s same-target retries and cross-provider fallback. The SDK builds it by parsing the `X-Mesh-Routing-Attempts` and `X-Mesh-Routing-Fallback` response headers, so it only appears when your API key has an active [routing policy](/docs/platform/retry-and-fallback). Which upstream provider served the request is internal and is not reported.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/resilience
