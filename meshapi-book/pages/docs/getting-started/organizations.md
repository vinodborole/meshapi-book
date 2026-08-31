---
type: Web Page
title: Organizations & Access Control - Mesh API
description: Manage teams, roles, and usage limits across your organization.
resource: https://developers.meshapi.ai/docs/getting-started/organizations
timestamp: '2026-08-31T13:14:57.224524+00:00'
---

[Dashboard](https://app.meshapi.ai).

## The hierarchy

Each user belongs to 

**one organization at a time**. To join a different organization, you leave your current one when accepting the new invitation.

## Roles

### Organization roles

### Team roles

Organization owners and admins automatically have admin access to every team in the organization.

## Inviting members

1

Send an invitation

An organization owner or admin invites a teammate by email from the Dashboard, choosing their role (admin or member) and, optionally, a team to add them to.

2

The invitee accepts

The invited person receives an email with a link. After signing in with the invited email address, they accept the invitation to join the organization with the assigned role.

3

Manage pending invitations

Admins can view, resend, or revoke pending invitations from the Dashboard. Invitations expire automatically after 72 hours.

## Usage limits & spend caps

Limits can be set at four scopes. Each scope supports the same set of controls:
The four scopes:

A request must satisfy 

**every**limit that applies to it — across the key, the member, the team, and the organization. If any one of them is exceeded, the request is rejected. Any scope left unset has no limit at that level.

Spend caps at the organization, team, and member levels are evaluated over a rolling 30-day window.

## When a limit is hit

Rate-limit responses include a 

`Retry-After` header indicating how long to wait before retrying. If a member or organization spend cap is reached, contact your organization admin to raise it.
## Managing orgs through the API

Org governance — invites, roles, teams, ownership — is a 

**dashboard**surface. Those endpoints authenticate a signed-in person and accept neither an`rsk_` key nor a `mak_` admin key, so there is no scripted equivalent. What automation *can*do: manage keys and limits with an[admin key](/docs/getting-started/api-keys), and read usage, spend, and audit logs with an`rsk_` key.
### The `/current` model

Every org endpoint is scoped to `/current` — the org the caller is signed in to. There is **no**:

`{org_id}` path parameter anywhere in the API
### Inviting people

Invite from
**Dashboard → Members**. Creating a member

*is*creating an invitation —

`POST /v1/orgs/current/members`, what that screen calls, returns an invitation record, and the person joins when they accept it.
The invitee accepts through `/v1/invitations` — they can validate a token, list invitations addressed to them, and accept one.
You cannot assign 

`owner` through an invitation, and you cannot change someone into an owner with a role update — ownership moves only through `POST /v1/orgs/current/transfer-ownership`. Attempting either returns `422`.
## Shared billing

`GET /v1/orgs/current/spend` breaks spend down by member, which is the fastest way to find out where the balance went.
## Pooled limits

Limits set at org, team, and member level combine with the key’s own limits, and
**the smallest applicable value wins**. Raising a key’s limit does nothing if the team above it is lower.

`GET /v1/keys/{id}/limits` shows the resolved result and which tier is binding — see [Rate Limits & Spend Caps](/docs/getting-started/rate-limits).

## Audit log

Every governance action in your org — key created, member invited, role changed, limit updated — is recorded in an append-only audit trail.`rsk_` key inherits the role of whoever owns it.
`GET /v1/audit-logs.csv` returns the same filtered trail as a CSV download for compliance reviews.
## Related

- [API Keys](/docs/getting-started/api-keys) — attaching keys to an org or team
- [Rate Limits & Spend Caps](/docs/getting-started/rate-limits) — how pooled limits resolve
- [Usage & Monitoring API](/docs/reference/usage-api) — org-level usage and spend

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/organizations
