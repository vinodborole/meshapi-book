---
type: Web Page
title: Usage & Monitoring API - Mesh API
description: Pull your usage, spend, rate-limit, and balance data programmatically
  — the same numbers the dashboard shows.
resource: https://developers.meshapi.ai/docs/reference/usage-api
timestamp: '2026-08-31T13:14:57.224524+00:00'
---

**usage, spend, rate-limit, and balance data programmatically**. Each authenticates with an

`rsk_` API key, so anything you
can see in the console you can also pull from your own code.
Full request/response schemas and an interactive explorer are in the
the 

**API Reference**tab. This page is the guide.

CSV export (

`GET /v1/usage/events/export`) is dashboard-only — it authenticates
the signed-in session and is not callable with an API key. Forecasting / usage
prediction is not part of this API yet.
## Authentication & scoping

An API key can only ever read the usage of the key it authenticated with — the
scope is derived server-side and cannot be widened by request parameters. The
dashboard’s own views are org-scoped instead, which is why 

`org_id` and the
`org` / `team` / `member` scopes appear in these schemas; from your own code
they do not apply.
## Usage summary — `POST /v1/usage`

Aggregate requests, tokens, and spend, plus a per-model breakdown. Filters are a
JSON body (the array filters are more natural as a body than query params).
`by_model` is one page (`by_model_total` is the full
distinct-model count). Monetary values are strings to preserve decimal
precision.
## Per-request history — `POST /v1/usage/events`

Paginated list of individual requests, newest first. Same filters as the
summary, plus `limit` (≤ 200) / `offset`.
`routing_fallback`.
### Per-attempt timeline — `GET /v1/usage/events/{event_id}/attempts`

When a request was retried or rerouted, this returns every attempt that shares
its `request_id` — the served row plus each retried row, oldest first. It is how
you see every model that was tried behind one collapsed event.
## Spend trend — `GET /v1/usage/spend-trend`

Historical spend bucketed by `granularity`, plus a trailing 3-bucket moving
average and the window’s total.
## Rate limits — `GET /v1/usage/rate-limits`

Live RPM / RPD / TPM counters vs. the effective limits (always fresh, never
cached). For an API-key caller only the `key` scope is populated.
`current`, the effective `limit`, `pct` (0–100), and
`resets_in_seconds`. A limit the key does not explicitly set is shown as the
system default with `is_default_limit: true` (the TPM default is display-only
— keys with no explicit `tpm_limit` are not TPM-throttled today). Use `pct` to
draw a usage bar and warn as it nears 100. The dashboard sees the
`org` / `team` / `member` scopes instead of the single `key` scope.
## Balance — `GET /v1/balance`

Current credit balance, plus how much is **reserved**by in-flight operations (realtime sessions, async background jobs, video generation) and therefore not spendable.

**org owner’s**balance is returned — all org members share one billing pool; a personal key with no org gets its own balance.

`available_usd` = `max(0, balance_usd − reserved_usd)`; `reserved_breakdown`
lists only the categories currently holding funds and sums to `reserved_usd`.
## Notes

- **Money** is always a decimal string (never a float).
- **Caching:** API-key reads are computed fresh; the dashboard’s org-scoped
summary/events reads are cached ~60s (`?refresh=true` bypasses). Rate-limits
and balance are never cached.
- **Errors** use the standard envelope —`401` (bad credential),`403` (not an
org member),`429` (rate limited).

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/usage-api
