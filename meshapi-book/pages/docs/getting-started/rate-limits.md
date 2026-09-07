---
type: Web Page
title: Rate Limits & Spend Caps - Mesh API
description: How request, token, and spend limits are set across keys, teams, and
  orgs — and how MeshAPI resolves them when several apply at once.
resource: https://developers.meshapi.ai/docs/getting-started/rate-limits
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

Exceeding a rate limit returns 

`429`. Exceeding a spend cap or running out of credit returns `402`.
## Defaults

A key with nothing configured still has limits:
There are also hard ceilings you cannot exceed, whatever you configure: 

**RPM ≤ 1,000**and

**RPD ≤ 1,000,000**. Requests to set a key above these are rejected with

`422`.
## Where limits can be set

Limits exist at five scopes:
1. **Key** —`rpm_limit` ,`rpd_limit` ,`tpm_limit` ,`spend_cap_usd`
2. **Team member** — that person’s ceiling within one team
3. **Org member** — that person’s ceiling across the org
4. **Team**
5. **Organization**

**every request the key has ever made**. The scopes above it count only the

**last 30 days**, so spend there ages out on a rolling basis and there is no monthly reset date.

### Which person a member limit applies to

A member limit is attributed to
**whoever created the key**, not whoever sends the request. An inference request carries a key and nothing else, so the API cannot tell two people apart. The key is the identity. If several people share one key, all of their usage counts against the creator’s member limit, and the other people’s own limits do nothing at all. When that one limit runs out, the key returns

`402` for everyone holding it.
A key also keeps counting against its creator’s limit after that person leaves
your organization, until someone revokes the key.
### Minimum wins

When more than one scope applies,
**the smallest value wins**— limits restrict, they never raise. A key with 500 RPM inside a team capped at 100 RPM is effectively limited to 100. This means raising a key’s limit has no effect if a tier above it is lower. To actually widen a limit you must raise every tier that binds.

### Checking the resolved value

Rather than working it out by hand, ask the API:`effective_*` values after the minimum-wins resolution, along with the org, team, and member context that produced them — so you can see *which*tier is binding.

This endpoint takes an 

**admin key**(`mak_...`), not an `rsk_` key — key management and inference are separate credentials. Create one in **Dashboard → Admin Keys**; see[API Keys](/docs/getting-started/api-keys). The same resolved limits are shown on the key’s page in the dashboard.
## Request body size

Every endpoint has a hard ceiling of
**32 MiB (33,554,432 bytes)**on the total request body, independent of your key, team, or org configuration. It is not raisable. An oversized request is rejected

**at the edge, before it reaches MeshAPI**. That has consequences worth knowing, because it does not look like the other errors on this page:

- the response is a plain HTML `413 Request Entity Too Large` page,**not** a Mesh
JSON error envelope — SDKs and error parsers will fail to decode it
- there is **no `request_id`** , and the request does not appear in your usage or
error logs
- it is **not billed** , and it does not consume RPM/RPD

`/v1/chat/completions` or
`/v1/responses`, reference images in `/v1/images/generations`, audio or video inputs.
Base64 inflates a payload by roughly **33%**, so a single

**~24 MB**file already exceeds the limit once encoded. To stay under it:

- send a **public URL** instead of Base64 wherever the endpoint accepts one
- for large files, upload via the [Files API](/docs/capabilities/rag) and reference the
returned file ID
- downscale or re-compress images before encoding — vision models gain nothing from a 20 MB source image

## Watching your usage against limits

`GET /v1/usage/rate-limits` reports current consumption against your limits — see the [Usage & Monitoring API](/docs/reference/usage-api).

## Behaviours worth knowing

**A spend cap can be exceeded by one request.**Billing settles

*after*a response is produced, because the true token count isn’t known until then. The final request that crosses the cap completes and is charged; the next one is rejected. For the same reason your balance can go slightly negative. Treat the cap as “stop shortly after this”, not a hard transactional ceiling.

**Free models still consume rate limits.**A model priced at zero costs nothing against your balance or spend cap, but it consumes RPM and RPD like any other request.

**Memory injection is checked twice.**Attaching a memory grows the prompt, so the spend cap is re-evaluated against the assembled request. A key near its cap can be rejected once memory is attached even though the raw request would have passed. See

[Memory](/docs/capabilities/memory).

**Restricting a key to a model list is an access control, not a limit — but it silently breaks features that route to models outside the list. See**

`allowed_models` interacts with limits in a surprising way.
[API Keys](/docs/getting-started/api-keys).

## Related

- [API Keys](/docs/getting-started/api-keys) — setting limits and caps on a key
- [Usage & Monitoring API](/docs/reference/usage-api) — reading current consumption
- [Account Configuration Checklist](/docs/getting-started/account-checklist) — the setup pass over every one of these settings

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/rate-limits
