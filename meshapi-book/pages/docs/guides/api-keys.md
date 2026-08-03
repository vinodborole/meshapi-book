---
type: Web Page
title: API Keys | Mesh API Docs
description: Create and manage inference keys programmatically — labels, limits, spend
  caps, model allow-lists, and org/team assignment.
resource: https://developers.meshapi.ai/docs/guides/api-keys
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# API Keys

An API key (`rsk_...`) is how your applications authenticate to MeshAPI. It is also the unit that carries **limits, spend caps, model access, and usage attribution** — so managing keys is how you control what each application is allowed to do.

This page covers the key-management API. For *using* a key to authenticate a request, see [Authentication](/docs/guides/authentication).

Key management uses a **user JWT** — your dashboard session token — not an `rsk_` key. An inference key cannot create or modify keys, by design: a leaked key must not be able to mint more.

## Creating a key

The plaintext key is returned **once**, in the `key` field of the create response, and is never retrievable again — only a hash is stored. Capture it at creation or you will have to issue a new one.

### Fields

`rpm` above 1,000 or `rpd` above 1,000,000 are rejected with `422`.

## Managing keys

`DELETE` is a **soft delete**: it sets the key’s status to `suspended`. The key stops working at the inference layer immediately, but the record and its usage history are preserved. There is no hard-delete endpoint — this keeps historical usage attributable.

## Model allow-lists

`allowed_models` restricts a key to a named set of models. It is the cleanest way to stop a cheap application reaching an expensive model.

An allow-list silently breaks features that route to models you didn’t list:

- **`model: "auto"`** — the Auto Router picks from your allow-list. If none of its candidates are listed, routing fails.
- **Web search** —`/v1/web/search` checks against its own model pin. An allow-list that omits it disables web search for that key.

Neither failure is obvious from the error. If a feature stops working right after you set an allow-list, this is why. Include the models those features depend on, or leave the key unrestricted.

## Org and team keys

Assigning `org_id` or `team_id` makes a key part of a shared structure: it draws on the **org’s shared balance** and inherits the org’s and team’s limits, resolved minimum-wins alongside the key’s own.

Attaching a `routing_policy_template_id` requires the key to belong to an org — templates are org-scoped, and a key with no org gets a `422`.

See [Organizations & Teams](/docs/guides/organizations-teams) for the surrounding model.

## Related

- [Authentication](/docs/guides/authentication) — using a key to make requests
- [Rate Limits & Spend Caps](/docs/guides/rate-limits-spend-caps) — how limits resolve across tiers
- [Organizations & Teams](/docs/guides/organizations-teams) — shared billing and pooled limits
- [Account Configuration Checklist](/docs/guides/account-configuration-checklist) — recommended setup pass

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/api-keys
