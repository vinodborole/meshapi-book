---
type: Web Page
title: Error Reference - Mesh API
description: Every error code the API returns, which side of the call it belongs to,
  and what to do about it.
resource: https://developers.meshapi.ai/docs/reference/errors
timestamp: '2026-09-21T12:28:28.555532+00:00'
---

`error.code` is stable and safe to branch on; `message` is for humans and may change.
```
{
  "error": {
    "code": "context_window_exceeded",
    "message": "This model's maximum context length is 200000 tokens.",
    "provider_code": "context_window_exceeded"
  },
  "request_id": "req_01JQ..."
}
```
`code` is always present. `provider_code` appears when the failure came from a model provider and carries the more specific reason — branch on `code` first, then narrow on `provider_code` if you need to.
Always log `request_id`. It is the only handle that ties your failure to our logs, and support cannot trace a report without it.
## What to do about it

Every error falls into one of these classes. The class tells you where the remedy lies; the row then names the specific action.
| Class | What it means | Typical status | 
|---|---|---|
| **Your request** | Something about the request itself was rejected. Retrying it unchanged will fail the same way. | `400` ,`403` ,`413` ,`422` — whichever the model provider returned. | 
| **Your account** | The request was well-formed, but your account’s own limits or balance stopped it. Nothing reached a model, and nothing was charged. | `401` unauthenticated,`402` out of credit or over a spend cap,`403` model not allowed,`429` rate limited. | 
| **Service-side failures** | The remedy differs per code — most clear on a retry with backoff, some need a different action. Each row says which. | `429` upstream throttle,`502` /`503` unavailable,`504` timed out,`500` ours. | 
| **Depends on the individual results** | One code covering several attempts. Which side is at fault is in the per-attempt detail in the response body, not in the code itself. | Whatever the underlying attempts returned. | 

## Your request

Something about the request itself was rejected. Retrying it unchanged will fail the same way.
| `error.code` | `provider_code` | What happened | What to do | 
|---|---|---|---|
| `bad_request` | — | The request was rejected before reaching a provider — missing required fields or an invalid combination of options. | Check required parameters and their combinations against the API reference, then retry. | 
| `conflict` | — | The request was well-formed, but the resource is already in a state that refuses it — granting a role the user already holds, or revoking the last super_admin. | Re-read the resource and retry against its actual state. This is not a payload problem: the same request may succeed later, or against a different resource. | 
| `content_policy_violation` | `content_policy_violation` | The provider refused the request because the prompt or generated content tripped its content filter. | Revise the prompt to comply with the provider’s usage policy, or try a model with a different moderation policy. | 
| `context_window_exceeded` | `context_window_exceeded` | The request’s input tokens (plus requested output) are larger than the model can accept. | Shorten the prompt, trim conversation history, or switch to a model with a larger context window. | 
| `invalid_api_version` | — | The X-Mesh-Version header named a version this API does not serve, was malformed, or was sent twice with different values. | Call GET /v1/api-versions for the supported labels and send exactly one of them, or omit the header to get the baseline version. | 
| `invalid_input` | `invalid_input` | An attached input (image, audio, file, or data URL) was corrupt, an unsupported format, or too large. | Verify the file is a supported format and within size limits, then re-encode or re-upload it. | 
| `invalid_request` | `invalid_request` | One or more request parameters were malformed, out of range, or unsupported by the model. | Check the request body against the API reference — verify parameter names, types, and value ranges. | 
| `invalid_request_error` | — | The request body was rejected before handling — a required field is missing, the wrong type, or malformed JSON. Returned by /v1/messages, which uses the Anthropic error envelope and answers schema problems with 400 rather than 422. | Read the message for the offending field path (for example ‘max_tokens: Field required’), correct the payload, and retry. | 
| `model_capability_not_supported` | — | The chosen model cannot serve the requested endpoint/capability (e.g. a chat-only model asked to embed or generate images), or cannot serve the service_tier the request asked for. | Use an endpoint the model supports, or pick a model whose capabilities match the request. A service_tier refusal names the model: call GET /v1/models and pick one that publishes a price for that tier. See [Service Tiers](/docs/capabilities/service-tiers) . | 
| `model_not_available_on_pinned_provider` | — | This API key is pinned to a single provider (fixed_provider) and that provider does not serve the requested model. The pin is deliberate — the request is refused rather than routed elsewhere. | The error’s details name the pinned provider and the model. Request a model that provider offers, call it on an unpinned key, or change the key’s fixed_provider. | 
| `model_not_found` | `model_not_found` | The requested model id is unknown to the gateway, disabled, or not permitted for this API key. | Call GET /v1/models for the exact enabled model ids, and confirm the key’s allowed_models includes it. | 
| `not_found` | — | The requested Mesh resource (key, template, batch, etc.) does not exist or is not visible to this key. | Verify the resource id and that it belongs to the same org/team as the key. | 
| `resource_not_found` | `resource_not_found` | A referenced resource (file id, batch id, previous response id, or fine-tune) does not exist or has expired. | Verify the id is correct and still valid; re-create the resource if it has expired. | 
| `unprocessable_entity` | — | The request was well-formed but semantically invalid — a generic user-input error with no more specific code. | Review the message detail and the request payload; correct the offending field and retry. | 
| `unsupported_operation` | `unsupported_operation` | The model or provider does not support the requested capability (e.g. tools, streaming, vision, this endpoint). | Remove the unsupported parameter/feature, or choose a model that supports it (see the model’s capabilities). | 
| `validation_error` | — | FastAPI/Pydantic rejected the request body before handling — a field is missing, the wrong type, or malformed JSON. | Inspect the ‘details’ array in the response for the exact field paths, fix them, and retry. | 

## Your account

The request was well-formed, but your account’s own limits or balance stopped it. Nothing reached a model, and nothing was charged.
| `error.code` | `provider_code` | What happened | What to do | 
|---|---|---|---|
| `concurrent_sessions_exceeded` | — | The account already has as many simultaneous realtime sessions as it is allowed. | Close an existing session before opening another, or request a higher concurrency limit. | 
| `feature_not_enabled` | — | The realtime API is not enabled for this deployment or account. | Contact support to have realtime enabled for your account. | 
| `forbidden` | — | The key is valid but not permitted to perform this action or reach this resource. | Use a key with the required permission, or request access from an org/team admin. | 
| `insufficient_credits` | `insufficient_credits` | This request used your own provider key, and that provider account has no remaining credit or has a billing problem. | Top up credits or fix the billing method on the provider account your key belongs to. | 
| `insufficient_quota` | — | The account behind this key has no remaining credit, so the realtime session was refused. | Add credits, then reconnect. | 
| `invalid_api_key` | — | The /v1/realtime socket was opened with a key that is missing, malformed, revoked or suspended. | Reconnect with an active rsk_ key; create or rotate keys in the dashboard. | 
| `rate_limit_exceeded` | — | The API key exceeded a limit configured on it in Mesh — requests per minute (RPM), requests per day (RPD), or tokens per minute (TPM). Set by you or an org admin, not by the model provider. | Back off and retry after the Retry-After interval, or raise that limit on the key in Settings -> API keys. The response message names which limit was hit. | 
| `session_token_cap_exceeded` | — | This realtime session used the maximum tokens allowed for a single session. | Open a new session to continue; raise the per-session cap if you need longer conversations. | 
| `spend_limit_exceeded` | — | The API key hit its configured USD spend cap for the period. | Increase the key’s spend limit or add credits to the account, then retry. | 
| `unauthorized` | — | The Mesh API key was missing, malformed, revoked, or does not exist. | Send a valid key as ‘Authorization: Bearer rsk_…’. Create or rotate keys in the dashboard. | 
| `upstream_auth_error` | `account_not_entitled` | This request used your own provider key. The credential is valid, but the provider is refusing the account it belongs to access to this model family. We did not fall back to a shared key, because the account is yours and only you can resolve it. | Check whether your account has access to this model family in the provider’s console. If it already shows as granted, the block is account-level and the provider’s support has to lift it — a console change will not clear it. | 
| `upstream_auth_error` | `invalid_auth` | This request used your own provider key, and the provider rejected that credential (401). | Verify your provider key is correct, active, and has access to the requested model; rotate it if compromised. | 
| `upstream_auth_error` | `permission_denied` | This request used your own provider key. The credential is valid but not authorized for this model or action (403). | Grant your provider key access to the requested model/region in the provider’s console, or use a key that has it. | 
| `upstream_rate_limit_error` | `monthly_quota_exceeded` | This request used your own provider key, and that account has used up its allotted monthly quota. | Wait for the quota to reset or raise the quota/plan in the provider’s console. | 
| `upstream_rate_limit_error` | `rate_limit_exceeded` | This request used your own provider key, and the provider throttled that credential (429). This is the provider’s limit, not a limit set on your Mesh key. | Slow the request rate, add backoff, or request a higher rate limit from the provider. | 

## Service-side failures

The remedy differs per code — most clear on a retry with backoff, some need a different action. Each row says which.
| `error.code` | `provider_code` | What happened | What to do | 
|---|---|---|---|
| `api_version_migration_failed` | — | Your request is pinned to an older API version and the gateway failed while converting the response back to that version’s shape. | Retry; if it persists, contact support with the request_id. Pinning to a newer version avoids the conversion entirely. | 
| `byok_fallback_disabled` | — | The request was routed to a provider you have configured your own API key for, that key could not serve it, and the key is set to forbid falling back to Mesh’s credentials. | Check your provider key’s status with the provider, or enable allow_fallback on it (PATCH /v1/provider-keys/) to let Mesh serve the request on its own credentials instead. | 
| `connection_timeout` | — | The gateway stopped receiving heartbeats on the socket and closed it, usually a network interruption. | Reconnect; if it recurs, check for a proxy or network path that drops idle WebSockets. | 
| `gateway_timeout` | `asset_registration_timeout` | A reference file supplied with the request was still being prepared upstream when the request ran out of time. Generation never started. | Retry the request — a reference that has finished preparing is reused, so the retry is faster. If it keeps timing out, try a smaller reference file. | 
| `gateway_timeout` | `provider_timeout` | The upstream provider did not return a response within the gateway’s timeout. | Retry the request; for long generations reduce max_tokens or enable streaming. If it persists, the provider is slow. | 
| `idle_timeout` | — | The realtime socket was idle for longer than the allowed window and was closed. Expected behaviour, not a failure. | Reconnect when you next need the session; send periodic activity to keep one open. | 
| `internal_error` | — | An unhandled exception occurred inside the gateway. | Retry once; if it persists, contact support with the request_id so we can trace the failure. | 
| `invalid_search_parameter` | — | A GET /v1/models/search filter or sort names something the catalog cannot honour — an unknown capability, or a price sort without exactly one priceable output modality. | Read the valid values from the response’s `facets` , and pair`sort=price` with exactly one`output_modality` of text, image or video. | 
| `model_brand_unknown` | — | A model policy — set on your organization, a team, your own account, or the API key — covers whole brands, and the requested model has no brand recorded, so the policy cannot be applied to it. | Call a model you are permitted to use, or contact support to have this model’s brand recorded. | 
| `model_excluded` | — | A model policy does not allow the requested model — it is outside the permitted models, or explicitly excluded. The policy can be set on your organization, a team, your own account, or the API key, and every one of them must permit a model. | Call a model you are permitted to use, or ask your organization admin to allow this one. | 
| `model_policy_unavailable` | — | A model policy — set on your organization, a team, your own account, or the API key — restricts which models may be called, and that policy could not be evaluated for this request. | Retry in a few seconds. If it persists, contact support. | 
| `pii_redaction_unavailable` | — | Your key or organization has PII redaction switched on, and the redaction pass could not complete for this request — most often because the request’s total text exceeded the scan ceiling. The request was refused BEFORE reaching the model provider: nothing was sent, and nothing was billed. This is deliberate. Redaction protects your data, and forwarding a request unredacted because the check failed would send personal data to a third party irreversibly. | Split the request into smaller ones, or reduce the amount of text it carries. If you would rather such requests go through UNREDACTED instead of being refused, set “on_error”: “allow” on the policy — that is an explicit, audited choice to accept the exposure. Retrying the same request unchanged will fail the same way. | 
| `provider_error` | — | The upstream realtime provider returned an error mid-session. | Reconnect; if it repeats, report it with the request_id. | 
| `provider_job_failed` | — | The request reached work the provider charges for and no usable result came back — a job that failed or returned nothing, a create whose outcome we could not confirm, or one we could not record. Served as 500 deliberately: 502/503/504 are back-off signals every SDK retries on, and a repeat can start a second job. The provider’s own message is forwarded ahead of this explanation when it sent one. | You were not charged for this attempt. Send the request again if you want a new job; it will be charged only if it succeeds. | 
| `provider_not_available` | — | The model’s provider is configured but its adapter is not registered (missing platform credentials) or a circuit breaker is open. | This is a server-side configuration/health issue, not your request. Retry later or use another model; escalate if it persists. | 
| `provider_resource_not_found` | — | The video provider no longer recognises this task id — typically because it expired on their side before it was collected. | Submit the generation again; poll and download results promptly. | 
| `secret_provision_failed` | — | We could not write the provider credential you supplied to our secret store, so the key was not registered. Nothing was saved, and no existing key was changed. | Retry the request. If it persists, contact support with the request_id. | 
| `service_draining` | — | The instance serving your socket is being drained for a deploy, so the session was closed cleanly. | Reconnect — a new instance will accept the session immediately. | 
| `session_duration_exceeded` | — | The realtime socket hit the maximum lifetime for a single session and was closed. | Reconnect to continue; sessions are bounded by design. | 
| `upstream_error` | `bad_response` | The provider returned HTTP 200 but a body the gateway could not parse (truncated or malformed). | Retry the request. If it reproduces on a specific model, report it with the request_id so we can inspect the raw body. | 
| `upstream_error` | `connection_error` | A network, DNS, or TLS failure prevented the gateway from connecting to the upstream provider. | Retry shortly. If it persists across models, check the incident channel. | 
| `upstream_error` | `internal_provider_error` | The upstream provider returned a server-side error. | Retry with backoff. If it persists across models, check the provider’s status page and our incident channel. | 
| `upstream_error` | `provider_overloaded` | The upstream provider reported it is overloaded / at capacity (e.g. Anthropic 529, provider 503). A request that asked for a non-standard service_tier reports this when that tier’s capacity is full. You are not charged for it. | Retry with exponential backoff, or route to an alternate model/provider for this request. On a request that asked for a non-standard service_tier you can also retry without it to take standard capacity. See [Service Tiers](/docs/capabilities/service-tiers) . | 
| `upstream_error` | `unknown` | The provider failed with an error the gateway could not map to a specific category. | Retry with backoff and report the request_id — an unclassified error usually needs a new classifier entry. | 
| `upstream_overload` | — | The upstream provider refused the session because it is overloaded. | Retry with backoff, or try a different realtime model. | 

## Depends on the individual results

One code covering several attempts. Which side is at fault is in the per-attempt detail in the response body, not in the code itself.
| `error.code` | `provider_code` | What happened | What to do | 
|---|---|---|---|
| `all_models_failed` | — | POST /v1/compare fans out to several models and none returned a response, so there is nothing to compare or synthesise. This code says only that every attempt failed — WHY is in the per-model errors. | Read the per-model errors in the response body. If they name something in your request, fix that and retry; if they all report the provider unavailable, retry with backoff or narrow the model list. | 

## Where it failed

Failed requests in the dashboard logs carry a`failure_stage` — how far the request got before it stopped. Successful requests have none.
| Stage | Meaning | 
|---|---|
| `auth` | Authenticating the API key. | 
| `guard_rate_limit` | Checking your requests-per-minute / per-day limits. | 
| `guard_spend_cap` | Checking your USD spend caps. | 
| `guard_model_access` | Checking the key is allowed to call this model. | 
| `template` | Resolving and rendering a prompt template. | 
| `pii_redaction` | Removing personal data from the request before sending it. | 
| `balance` | Checking the account’s credit balance. | 
| `credentials` | Resolving the upstream credential for the model. | 
| `upstream_submit` | Sending the request to the model provider. | 
| `upstream_stream` | Reading the streamed response back. | 
| `settlement` | Recording usage and settling the charge. | 

## What errors never contain

Messages are stripped of upstream internals before they reach you — SDK call frames, cloud resource identifiers and anything credential-shaped. Two consequences worth knowing:
- **Which provider served a request is not disclosed.** A model is reachable through more than one upstream and we may fail over between them, so the provider is not part of the contract.`X-Mesh-Routing-*` response headers tell you*that* a fallback happened, not to where.
- **A problem with our own upstream account reads as a provider outage.** If one of our provider credentials is rejected or throttled, you get`upstream_error` with a generic message rather than the specific cause. When you use your own provider key (BYOK) the real error is forwarded to you, because the account is yours to fix.

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/errors
