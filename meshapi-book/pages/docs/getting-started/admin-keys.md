---
type: Web Page
title: Admin Keys - Mesh API
description: The mak_ credential a script uses to manage your organization — permissions,
  scopes, expiry, rotation, and revocation.
resource: https://developers.meshapi.ai/docs/getting-started/admin-keys
timestamp: '2026-08-31T13:14:57.224524+00:00'
---

`mak_...`) lets a script manage your organization — creating API keys, reading resolved limits, configuring alerts — without a person signing in. It is deliberately **not**the key you already have.

The separation is enforced twice server-side, by credential prefix and by separate storage. A leaked inference key cannot mint more keys, and a leaked admin key cannot spend your balance.

Admin keys are minted from the dashboard by a signed-in person — 

**Dashboard → Admin Keys**. An admin key cannot mint another admin key, by design.
## What an admin key can reach

A key carries an explicit permission set. Anything not granted is a`403`, and anything not in this table is out of reach for **every**admin key:

## Scope

Scope decides
*whose*rows a permission reaches:

`alerts:*`, `limits:*`, and `org:read` act on the organization itself, so a narrower scope would select the same rows while reading as a restriction. They can only be granted at scope `org` — asking for them at `self` or `team` is a `422`. Only `keys:read` and `keys:write` are meaningful below org scope.`403`, and so is a scope that reaches further than yours. A team admin can mint a `team`-scoped key for their own team, not for another.
## Creating one

Create one in
**Dashboard → Admin Keys**. The form maps onto

`POST /v1/admin-keys`, which authenticates the signed-in person — there is no credential that can mint an admin key, so this step is always a human one:
## Using it

The same`Authorization` header your API key uses:
## What the keys it creates belong to

An inference key minted by an admin key is owned by the
**admin key’s lineage**, not by a person. That has three consequences worth knowing before you automate anything:

- It lands in the admin key’s own organization. Sending a different `org_id` is a`403` ; sending a different`team_id` works only if the key’s scope reaches that team, and a`self` -scoped key can only create in the org’s default team.
- It bills the organization’s shared balance, like any other org key — see [Organizations & Teams](/docs/getting-started/organizations) .
- It survives rotation of the admin key that created it, because the lineage is what owns it. Replacing an admin key by minting a fresh one instead does **not** carry those keys over.

## Rotating

Rotate from
**Dashboard → Admin Keys**(

`POST /v1/admin-keys/{key_id}/rotate`, optionally with `{"grace_hours": 24}`). Like minting, it is a signed-in action — a key cannot rotate itself.
Rotation mints a successor and shortens the predecessor to a grace window — **both keys work during the overlap**, which is what lets you redeploy without a gap.

`grace_hours` defaults to 24 and is capped at 7 days.
The successor inherits the predecessor’s name, permissions, scope, and team. It also inherits its **lifetime**, not the 90-day ceiling: rotating a deliberately short-lived key does not quietly turn it into a long-lived one. Pass

`expires_at` to set a different one, subject to the same 90-day rule.
## Revoking

Revoke from
**Dashboard → Admin Keys**(

`DELETE /v1/admin-keys/{key_id}`, optionally with a `reason`).
Revocation is **immediate and has no grace window**— that is the difference from rotation.

`reason` is optional and is recorded on the key and in the audit trail.
## Listing

`GET /v1/admin-keys` and `GET /v1/admin-keys/{key_id}` return the masked key, permissions, scope, expiry, `last_used_at`, and who created it — never the plaintext. What you see depends on your own scope: org-wide access lists every key in the org, team access lists your team’s keys plus your own, and anything narrower lists only the keys you created yourself.
## Errors

## Related

- [Authentication](/docs/getting-started/authentication) —`rsk_` vs`mak_` , and using a key to make requests
- [API Keys](/docs/getting-started/api-keys) — the management API this credential is for
- [Rate Limits & Spend Caps](/docs/getting-started/rate-limits) — resolving limits across tiers
- [Organizations & Teams](/docs/getting-started/organizations) — shared billing, teams, and what stays a dashboard action

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/admin-keys
