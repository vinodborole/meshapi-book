---
type: Web Page
title: Error Reference - Mesh API
description: Every error code the API returns, which side of the call it belongs to,
  and what to do about it.
resource: https://developers.meshapi.ai/docs/reference/errors
timestamp: '2026-08-31T13:14:57.224524+00:00'
---

`error.code` is stable and safe to branch on; `message` is for humans and may change.
`code` is always present. `provider_code` appears when the failure came from a model provider and carries the more specific reason — branch on `code` first, then narrow on `provider_code` if you need to.
Always log `request_id`. It is the only handle that ties your failure to our logs, and support cannot trace a report without it.
## Whose fault was it

Errors fall into three classes. The class tells you whether to fix your request, fix your account, or retry.
## Your request

Something about the request itself was rejected. Retrying it unchanged will fail the same way.
## Your account

The request was well-formed, but your account’s own limits or balance stopped it. Nothing reached a model, and nothing was charged.
## Us or the model provider

Nothing was wrong with your request or your account. Safe to retry with backoff — and often already retried for you before you saw this.
## Depends on the individual results

One code covering several attempts. Which side is at fault is in the per-attempt detail in the response body, not in the code itself.
## Where it failed

Failed requests in the dashboard logs carry a`failure_stage` — how far the request got before it stopped. Successful requests have none.
## What errors never contain

Messages are stripped of upstream internals before they reach you — SDK call frames, cloud resource identifiers and anything credential-shaped. Two consequences worth knowing:
- **Which provider served a request is not disclosed.** A model is reachable through more than one upstream and we may fail over between them, so the provider is not part of the contract.`X-Mesh-Routing-*` response headers tell you*that* a fallback happened, not to where.
- **A problem with our own upstream account reads as a provider outage.** If one of our provider credentials is rejected or throttled, you get`upstream_error` with a generic message rather than the specific cause. When you use your own provider key (BYOK) the real error is forwarded to you, because the account is yours to fix.

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/errors
