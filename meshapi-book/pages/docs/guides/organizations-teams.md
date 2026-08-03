---
type: Web Page
title: Organizations & Teams | Mesh API Docs
description: Share billing, pool limits, and manage who can do what — organizations,
  teams, members, and invitations.
resource: https://developers.meshapi.ai/docs/guides/organizations-teams
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Organizations & Teams

Organizations & Teams

An **organization** is the billing and governance boundary for a company using MeshAPI. Inside it, **teams** group people and keys, and **members** hold roles that determine what they can change.

Individuals do not need an org — a personal account works fine. Create one when you need shared billing, per-person limits, or more than one person managing keys.

Org management uses a **user JWT**, not an `rsk_` key.

## The `/current` model

Every org endpoint is scoped to `/current` — the org identified by your session token. There is **no `{org_id}` path parameter anywhere in the API**:

Because the target org comes from your **token**, a stale token silently acts on the wrong org rather than erroring. If you belong to more than one org, re-authenticate after switching rather than reusing a cached token.

## Roles

You cannot assign `owner` through an invitation, and you cannot change someone into an owner with a role update — ownership moves only through `POST /v1/orgs/current/transfer-ownership`. Attempting either returns `422`.

## Inviting people

Creating a member *is* creating an invitation — `POST /v1/orgs/current/members` returns an invitation record, and the person joins when they accept it.

The invitee accepts through `/v1/invitations` — they can validate a token, list invitations addressed to them, and accept one.

## Shared billing

**Every member of an org spends from one balance — the owner’s.** There is no per-member wallet. A single member with an uncapped key can consume the entire organization’s credit.

Per-member spend caps are the control for this, and they are **unlimited by default**. Set them deliberately; see [Rate Limits & Spend Caps](/docs/guides/rate-limits-spend-caps).

`GET /v1/orgs/current/spend` breaks spend down by member, which is the fastest way to find out where the balance went.

## Pooled limits

Limits set at org, team, and member level combine with the key’s own limits, and **the smallest applicable value wins**. Raising a key’s limit does nothing if the team above it is lower.

`GET /v1/keys/{id}/limits` shows the resolved result and which tier is binding — see [Rate Limits & Spend Caps](/docs/guides/rate-limits-spend-caps).

## Related

- [API Keys](/docs/guides/api-keys) — attaching keys to an org or team
- [Rate Limits & Spend Caps](/docs/guides/rate-limits-spend-caps) — how pooled limits resolve
- [Usage & Monitoring API](/docs/guides/usage-monitoring-api) — org-level usage and spend

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/organizations-teams
