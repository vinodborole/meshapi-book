---
type: Web Page
title: Mesh API | Mesh API Docs
description: Diagnose and fix the most common issues when calling the Mesh API.
resource: https://developers.meshapi.ai/debug/mesh-api
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Mesh API

Most failures fall into one of a few buckets: auth, limits, bad requests, or a transient upstream error. This page maps the symptom you see to its cause and fix. Start with the first-response checklist, then jump to the section that matches your error.

Every request has a unique `req_...` ID returned in the response headers and
visible in Dashboard → **Logs**. Include it in any support message — it lets us
trace the exact call.

## First-response checklist

Before digging in, confirm the basics:

- **Base URL**— requests go to- `https://api.meshapi.ai`.
- **Auth header**—- `Authorization: Bearer rsk_...`(note the- `Bearer`prefix and the- `rsk_`key prefix).
- **Key is active**— check Dashboard →- **API Keys**; a deactivated key returns- `403`.
- **Balance**— a zero balance or hit spend cap returns- `402`. Check Dashboard →- **Billing**.
- **Model exists**— confirm the model ID is enabled via- `GET /v1/models`.
- **Read**— the response body’s- `error.code`- `error.code`field names the exact failure.

## Error code reference

Every error response carries a machine-readable `error.code` and a human
message:

## Reading an error: response vs. logs

Every failed request surfaces in **two** places that share one `request_id` —
match them to trace exactly what happened. Here’s the same failure, a `429`
rate limit, seen from both sides.

**1. What your app receives** — the HTTP response body your code catches:

The same ID is also returned in the `x-request-id` response header, so you can
log it even before parsing the body.

**2. What you see in Dashboard → Logs** — the matching row at
app.meshapi.ai → **Logs**:

Filter Logs by **Status: error** to list only failed calls, then open a row for
its full error code and message. The `req_...` ID is the link between what your
app saw and what we can trace — include it in any support message.

## Feature-specific debugging

Some features have their own gotchas. If your issue is with one of these, start on its page:

- BYOK — provider key config, permissions, fallback, fees.
- Structured Output — `response_format`not enforced, JSON parsing.
- Auto Routing — which model ran, classifier cost, latency.
- Video Generation — async polling, task failures, input limits.

## Common problems

### 401 unauthorized — my key is rejected

The key isn’t reaching the server correctly. Check, in order:

- Header is exactly `Authorization: Bearer rsk_...`— the`Bearer`prefix is required.
- No stray whitespace or newline in the key (common when read from a file).
- The env var is actually loaded — print its length, not its value, to confirm it isn’t empty.
- You’re using an account key (`rsk_`), not a provider key.

If a key may have leaked, deactivate it in Dashboard → **API Keys** and issue a
new one immediately.

### 403 forbidden — key is valid but request is denied

The key authenticated but isn’t allowed to do this. Usual causes:

- The key was **deactivated**or**suspended**— check its status in the dashboard.
- The key lacks scope for the endpoint you’re calling.
- Your account is on hold for billing — see **Billing**.

### 402 spend_limit_exceeded — payment required

Your balance is zero or a spend cap was reached.

- Check **Dashboard → Billing**for current balance.
- Top up, or raise the monthly/total spend cap on the key.
- Spend caps are per-key — a shared account can have one key blocked while others still work.

### 429 rate_limit_exceeded — too many requests

You hit a rate limit. Two kinds exist:

- **Your key’s limits**(- `rate_limit_exceeded`) — RPM, RPD, or TPM. Raise them in Dashboard →- **API Keys**, or slow down.
- **Provider throttling**(- `upstream_rate_limit_error`) — the upstream provider throttled us. Retry with backoff.

Respect the `Retry-After` header when present. All official SDKs retry `429`
automatically with exponential backoff and jitter.

### 404 model_not_found — model unavailable

The model ID is wrong or not enabled for your account.

- List what’s available: `GET /v1/models`.
- Model IDs are case-sensitive and provider-prefixed (e.g. `openai/gpt-4o`).
- Some models require BYOK or must be enabled first.

### 422 validation_error — bad request body

The request was well-formed JSON but a field is invalid. The `error.details`
field names the offending field.

Common culprits:

- Missing required field (`model`,`messages`).
- Wrong type — e.g. `temperature`as a string instead of a number.
- Out-of-range value — e.g. `max_tokens`above the model’s context window.
- `response_format`/ structured-output schema is malformed. See Structured Output.

### Sampling params rejected — temperature / top_p (upstream_error or 422)

Not every model accepts every sampling parameter, and the provider — not Mesh —
enforces this. A rejected parameter surfaces as an `upstream_error` (`500`) or a
`validation_error` (`422`).

- **Reasoning models**(e.g. OpenAI’s o-series) reject or ignore- `temperature`and- `top_p`. Drop them and steer with- `reasoning_effort`instead.
- **Some models reject**— pick one sampling strategy, not both.- `temperature`and- `top_p`sent together
- Out-of-range values (`temperature`outside`0–2`,`top_p`outside`0–1`) are also rejected.

When in doubt, send only `temperature` **or** `top_p`, and omit both for
reasoning models.

### 5xx upstream_error / gateway_timeout — server-side failure

These come from the upstream provider or a timeout, and are almost always transient.

- **Retry with exponential backoff**— the SDKs do this for- `500`,- `502`,- `503`,- `504`.
- If one model fails repeatedly, try Auto Routing or a different model.
- Persistent `provider_not_available`means no provider can serve that model right now — pick another.

### Streaming stops or drops mid-response

Streams do **not** auto-retry. A dropped connection raises an error with
`error_code="stream_interrupted"` (`MeshAPIError` in the SDKs).

- Catch it and restart a fresh request — partial output cannot be resumed.
- Mid-stream provider errors arrive as SSE frames before `[DONE]`and raise the same error type.
- Behind a proxy or load balancer, disable response buffering so tokens flush in real time.

### Requests hang or time out

- Raise the client `timeout`— reasoning and long-generation calls can take tens of seconds.
- Check network egress: the API is reachable at `https://api.meshapi.ai:443`.
- Streaming feels stuck? A buffering proxy is the usual cause — see the streaming section above.

### CORS errors in the browser

Calling the API directly from browser or mobile client code exposes your `rsk_`
key and triggers CORS failures.

**Never call Mesh API from client-side code.** Route requests through your own
backend proxy that holds the key. See Authentication → Security Best
Practices.

### Responses are slow or latency is high

- First token latency depends on the model and provider load — try a faster model or Auto Routing.
- Use **streaming**so users see tokens as they generate instead of waiting for the full response.
- For bulk, non-interactive work use the Batch API — cheaper and built for throughput.
- Large RAG contexts add latency; trim retrieved chunks. See RAG.

### Unexpected charges or usage

- Every call is logged with its cost in Dashboard → **Logs**. Filter by key, model, or date.
- Set **spend caps**per key to bound cost.
- Streaming and non-streaming are billed identically — on tokens used.

## Handling errors in code

All official SDKs raise a typed error carrying the status, error code, and request ID:

###### Python

###### Node.js

###### Go

## Still stuck?

Email **contact@meshapi.ai** with:

- The **request ID**(`req_...`) from the response headers or Dashboard logs.
- The **model**and**endpoint**you called.
- The **error code and message**you received.
- The **account email**tied to your API key.

More options on the Support page.

# Citations

1. Source page: https://developers.meshapi.ai/debug/mesh-api
