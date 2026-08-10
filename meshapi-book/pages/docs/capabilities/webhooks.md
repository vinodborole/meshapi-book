---
type: Web Page
title: Webhooks - Mesh API
description: Subscribe to account events and receive signed, retried HTTP callbacks
  in real time.
resource: https://developers.meshapi.ai/docs/capabilities/webhooks
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST` the moment they happen, instead of polling the API or the dashboard.
Every delivery is signed with an HMAC so you can verify it came from Mesh, retried on failure with exponential backoff, and logged so you can inspect or manually redeliver any attempt.
## Setting up a webhook endpoint

1

Register your endpoint

**Response:**

`destination` must be a publicly resolvable `http(s)` URL — not `localhost`, and not an address that resolves to a private, loopback, or link-local range. This is checked when you register or update the endpoint, and again before every delivery attempt, so an endpoint that later starts resolving to an internal address stops receiving deliveries rather than silently forwarding them there.
2

Subscribe to event types

Each subscription is one Some events are threshold-based. Every other event type takes no threshold — pass neither field. See 

`(event type → your endpoint)` pair. Create one policy per event type you want:`balance.low` takes a USD floor:`spend_cap.approaching` and `rate_limit.threshold` take a percentage instead:[Available events](#available-events)for which is which; the API rejects a threshold supplied for a no-threshold event, and rejects a missing one for a threshold event.
3

Verify deliveries

Once subscribed, matching events start arriving at your endpoint as signed 

`POST` requests. See [Verifying webhook signatures](#verifying-webhook-signatures)below before you trust any payload.
## Available events

Every delivery body is a JSON envelope:

`data` for each event:
A threshold event’s 

`data` carries the values it crossed with (e.g. `spent_usd`/`cap_usd`), not the specific `threshold_pct`/`threshold_usd` you subscribed with — the same event is fanned out to every matching subscription on your endpoint, and each subscription can have its own threshold, so the envelope can’t name “the” one that fired. If you have multiple subscriptions to the same event type at different thresholds, use `data` to compute which of yours applied.
`data` only ever contains fields your org already has access to — never a plaintext API key or a provider credential.
## Verifying webhook signatures

Every delivery carries these headers:
The signature is an HMAC-SHA256 of 

`{timestamp}.{raw_request_body}`, hex-encoded, using your endpoint’s signing secret. This is the same construction Stripe uses, so if you already have a verifier for another vendor, the shape will look familiar.
To verify a request:
1. Read the raw request body — **do not** re-serialize a parsed object. Any difference in key order, whitespace, or number formatting changes the bytes and the signature will not match.
2. Recompute the HMAC over `f"{timestamp}.{raw_body}"` using your signing secret.
3. Compare it against the `v1=` term(s) in`Mesh-Webhook-Signature` using a**constant-time** comparison — never`==` . A variable-time comparison leaks the correct signature one byte at a time through timing.
4. Reject the request if `Mesh-Webhook-Timestamp` is more than 5 minutes from your current time. The timestamp is inside the signed string, so an attacker can’t replay an old, captured request with a new timestamp — but without this check, an old captured request replayed with its*original* timestamp would still verify.

- Python
- Node.js
- Go

## Delivery semantics

- **At-least-once, never exactly-once.** The same event can arrive more than once — a retried attempt after a slow-but-successful response, or a manual redelivery.`Mesh-Event-Id` is your idempotency key: dedupe on it before acting on an event a second time.
- **No ordering guarantee.** Deliveries for different events can arrive out of order. Order on the payload’s`created_at` , not on arrival time.
- **Success is 2xx only.** Any other response — including a redirect — counts as a failed attempt. Redirects are not followed.
- **Retries with backoff.** The first attempt fires immediately. On failure, up to 5 more attempts follow — roughly`30s, 2m, 10m, 1h, 6h` after the previous one (jittered ±20%) — for 6 attempts total. After the last one fails, the delivery is marked`dead` : it stays visible in your delivery log and can be redelivered manually, but is not retried automatically again.
- **Timeout.** Each attempt waits up to 10 seconds for your endpoint to respond.

## Inspecting deliveries

**Response:**

Redelivering creates a 

**new**delivery row with the same

`event_id` and payload — the original attempt’s history is never modified, so both remain in your log.
## Rotating your signing secret

**Response:**

`grace_until` (24 hours after rotation) — deliveries in that window are signed with **both**secrets (see the dual

`v1=` terms above), so you can update your stored secret and redeploy without dropping any deliveries in between. After the grace window, only the new secret verifies.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/webhooks
