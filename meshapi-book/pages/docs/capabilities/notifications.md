---
type: Web Page
title: Notifications - Mesh API
description: Subscribe to account events and receive them as signed HTTP callbacks
  or as email, in real time.
resource: https://developers.meshapi.ai/docs/capabilities/notifications
timestamp: '2026-08-24T07:07:57.677646+00:00'
---

**destinations**. A destination is either a

**webhook endpoint**— a URL of yours, which receives a signed HTTP

`POST` — or an **email recipient**, an address that receives a readable message. Both are configured through the same API and share one delivery log. Webhook deliveries are signed with an HMAC so you can verify they came from Mesh, retried on failure with exponential backoff, and logged so you can inspect or manually redeliver any attempt.

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
## Sending events to an email address

A destination does not have to be a service. An
**email recipient**receives the same events as a readable message — no receiver to build, no signature to verify.

1

Add the address

2

Ask us to send the confirmation

**single-use and expires in 24 hours**; re-requesting mints a new one and voids the previous.The token is never returned to you — only the recipient can confirm. That is the point: it means nobody can sign a colleague up for mail they did not agree to.

3

The recipient confirms

They follow the link. Until they do, 

`verified_at` on the channel stays
`null` and **no events from this catalogue are delivered to that address.**Changing a channel’s`destination` clears the confirmation — consent belongs
to the address, not to the record — so a re-pointed channel must confirm
again.`GET /alerts`, which lists every channel:
**Email recipients cannot take threshold events yet**—

`balance.low`,
`spend_cap.approaching` and `rate_limit.threshold` each need a value you choose,
and that is configured per webhook endpoint. Every other event in the catalogue
can go to an address.
## Available events

Every delivery body is a JSON envelope:

`data` for each event:
### Which limit fired: `scope`

Spend caps and rate limits are enforced at five scopes, not just per key. `spend_cap.*` and `rate_limit.threshold` therefore carry a `scope` discriminator and a `scope_id` naming the thing the limit is attached to:
`key_id` is always present and is `null` for every scope except `key`, so a receiver that filters on it keeps working and gets an explicit “this is not about one key” rather than a missing field.
`spend_cap.approaching` is emitted for the `key` scope only. It is computed from the per-key running total, which is the one counter with a before/after delta available at the moment the total changes; the hierarchy counters have no equivalent. `spend_cap.hit` covers all five scopes.`data` carries the values it crossed with (e.g. `spent_usd`/`cap_usd`), not the specific `threshold_pct`/`threshold_usd` you subscribed with — the same event is fanned out to every matching subscription on your endpoint, and each subscription can have its own threshold, so the envelope can’t name “the” one that fired. If you have multiple subscriptions to the same event type at different thresholds, use `data` to compute which of yours applied. Each subscribed threshold is debounced on its own, so a ladder (say 50%, 80% and 95% on one key) delivers every rung it crosses rather than only the first.
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
- **Success is 2xx only** (webhook endpoints). Any other response — including a redirect — counts as a failed attempt. Redirects are not followed.
- **Retries with backoff.** The first attempt fires immediately. On failure, up to 5 more attempts follow — roughly`30s, 2m, 10m, 1h, 6h` after the previous one (jittered ±20%) — for 6 attempts total. After the last one fails, the delivery is marked`dead` : it stays visible in your delivery log and can be redelivered manually, but is not retried automatically again.
- **Timeout.** Each attempt waits up to 10 seconds for your endpoint to respond.
- **For email recipients** , at-least-once, ordering and the retry schedule are the same. A`succeeded` email delivery means the message was**accepted for sending** — it is not a receipt. A message accepted and then bounced by the receiving server is not currently reflected in the log, so treat the log as “we sent it”, not “they got it”.

## Inspecting deliveries

**Response:**

Redelivering creates a 

**new**delivery row with the same

`event_id` and payload — the original attempt’s history is never modified, so both remain in your log.
### Sending a test event

**real delivery**: it is signed, retried, dead-lettered and logged exactly like a live event, because a test that took a different code path would not tell you whether your real events will arrive. Your receiver can tell it apart by

`test` on the payload:
`409`, and an email recipient
answers `422` — test events go to webhook endpoints.
## Rotating your signing secret

**Response:**

`grace_until` (24 hours after rotation) — deliveries in that window are signed with **both**secrets (see the dual

`v1=` terms above), so you can update your stored secret and redeploy without dropping any deliveries in between. After the grace window, only the new secret verifies.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/notifications
