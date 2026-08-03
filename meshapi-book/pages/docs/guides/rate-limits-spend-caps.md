---
type: Web Page
title: Rate Limits & Spend Caps | Mesh API Docs
description: How request, token, and spend limits are set across keys, teams, and
  orgs — and how MeshAPI resolves them when several apply at once.
resource: https://developers.meshapi.ai/docs/guides/rate-limits-spend-caps
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Rate Limits & Spend Caps

Rate Limits & Spend Caps

MeshAPI enforces four kinds of ceiling on every request:

Exceeding a rate limit returns `429`. Exceeding a spend cap or running out of credit returns `402`.

## Defaults

A key with nothing configured still has limits:

The spend cap is the one default that is *not* protective. An unset spend cap means **unlimited spend** against your balance, not zero. If you want a key to be unable to burn your whole balance, you must set `spend_cap_usd` explicitly.

There are also hard ceilings you cannot exceed, whatever you configure: **RPM ≤ 1,000** and **RPD ≤ 1,000,000**. Requests to set a key above these are rejected with `422`.

## Where limits can be set

Limits exist at five scopes:

1. **Key** —`rpm_limit` ,`rpd_limit` ,`tpm_limit` ,`spend_cap_usd`
2. **Team member** — that person’s ceiling within one team
3. **Org member** — that person’s ceiling across the org
4. **Team**
5. **Organization**

All of them default to unlimited except the key-level defaults above.

### Minimum wins

When more than one scope applies, **the smallest value wins** — limits restrict, they never raise. A key with 500 RPM inside a team capped at 100 RPM is effectively limited to 100.

This means raising a key’s limit has no effect if a tier above it is lower. To actually widen a limit you must raise every tier that binds.

### Checking the resolved value

Rather than working it out by hand, ask the API:

The response returns both the raw key-level values and the `effective_*` values after the minimum-wins resolution, along with the org, team, and member context that produced them — so you can see *which* tier is binding.

This endpoint takes a **user JWT**, not an `rsk_` key. See [API Keys](/docs/guides/api-keys).

## Watching your usage against limits

`GET /v1/usage/rate-limits` reports current consumption against your limits — see the [Usage & Monitoring API](/docs/guides/usage-monitoring-api).

## Behaviours worth knowing

**A spend cap can be exceeded by one request.** Billing settles *after* a response is produced, because the true token count isn’t known until then. The final request that crosses the cap completes and is charged; the next one is rejected. For the same reason your balance can go slightly negative. Treat the cap as “stop shortly after this”, not a hard transactional ceiling.

**Free models still consume rate limits.** A model priced at zero costs nothing against your balance or spend cap, but it consumes RPM and RPD like any other request.

**Memory injection is checked twice.** Attaching a memory grows the prompt, so the spend cap is re-evaluated against the assembled request. A key near its cap can be rejected once memory is attached even though the raw request would have passed. See [Memory](/docs/guides/memory).

**`allowed_models` interacts with limits in a surprising way.** Restricting a key to a model list is an access control, not a limit — but it silently breaks features that route to models outside the list. See [API Keys](/docs/guides/api-keys).

## Related

- [API Keys](/docs/guides/api-keys) — setting limits and caps on a key
- [Usage & Monitoring API](/docs/guides/usage-monitoring-api) — reading current consumption
- [Account Configuration Checklist](/docs/guides/account-configuration-checklist) — the setup pass over every one of these settings

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/rate-limits-spend-caps
