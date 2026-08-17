---
type: Web Page
title: API Keys - Mesh API
description: Create and manage inference keys programmatically — labels, limits, spend
  caps, model allow-lists, and org/team assignment.
resource: https://developers.meshapi.ai/docs/getting-started/api-keys
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

`rsk_...`) is how your applications authenticate to MeshAPI. It is also the unit that carries **limits, spend caps, model access, and usage attribution**— so managing keys is how you control what each application is allowed to do. This page covers the key-management API. For

*using*a key to authenticate a request, see

[Authentication](/docs/getting-started/authentication).

## Creating a key

### Fields

`rpm` above 1,000 or `rpd` above 1,000,000 are rejected with `422`.
## Managing keys

`DELETE` is a **soft delete**: it sets the key’s status to

`suspended`. The key stops working at the inference layer immediately, but the record and its usage history are preserved. There is no hard-delete endpoint — this keeps historical usage attributable.
## Pinning the API version

A key can carry an`api_version` — the dated contract every request made with it receives,
unless the request sends its own `X-Mesh-Version` header. Useful when the calling code is not
yours to change, or when a whole integration should sit on one version.
`null` to clear it. In the dashboard it is the key’s **API Version**field. See

[API versioning](/docs/reference/api-versioning)for what a version covers and which ones are served.

## Model allow-lists

`allowed_models` restricts a key to a named set of models. It is the cleanest way to stop a cheap application reaching an expensive model.
## Org and team keys

Assigning`org_id` or `team_id` makes a key part of a shared structure: it draws on the **org’s shared balance**and inherits the org’s and team’s limits, resolved minimum-wins alongside the key’s own. Attaching a

`routing_policy_template_id` requires the key to belong to an org — templates are org-scoped, and a key with no org gets a `422`.
See [Organizations & Teams](/docs/getting-started/organizations)for the surrounding model.

## Related

- [Authentication](/docs/getting-started/authentication) — using a key to make requests
- [Rate Limits & Spend Caps](/docs/getting-started/rate-limits) — how limits resolve across tiers
- [API Versioning](/docs/reference/api-versioning) — pinning a key to a dated contract
- [Organizations & Teams](/docs/getting-started/organizations) — shared billing and pooled limits
- [Account Configuration Checklist](/docs/getting-started/account-checklist) — recommended setup pass

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/api-keys
