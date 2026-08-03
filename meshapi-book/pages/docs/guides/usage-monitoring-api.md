---
type: Web Page
title: Usage & Monitoring API | Mesh API Docs
description: Pull your usage, spend, rate-limit, and balance data programmatically
  — the same numbers the dashboard shows.
resource: https://developers.meshapi.ai/docs/guides/usage-monitoring-api
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Usage & Monitoring API

Usage & Monitoring API

The usage endpoints expose **usage, spend, rate-limit, and balance data
programmatically** with your `rsk_` API key, so anything you can see in the
console you can also pull from your own code.

Full request/response schemas and an interactive explorer are in the
[API Reference](/api-reference). This page is the guide.

Spend-trend charts and CSV export are part of the dashboard only — they are not callable with an API key. Forecasting / usage prediction is not part of this API yet.

## Authentication & scoping

An API key can only ever read the usage of the key it authenticated with — the scope is derived server-side and cannot be widened by request parameters.

## Usage summary — `POST /v1/usage`

Aggregate requests, tokens, and spend, plus a per-model breakdown.

Totals span the full range; `by_model` is one page (`by_model_total` is the full
distinct-model count). Monetary values are strings to preserve decimal
precision.

## Per-request history — `POST /v1/usage/events`

Paginated list of individual requests, newest first. Same filters as the
summary, plus `limit` (≤ 200) / `offset`.

Retried “waste” attempts are collapsed — each request appears once (the served
attempt). Which upstream provider served a request is never exposed; a
cross-provider routing fallback surfaces only as the boolean `routing_fallback`.

## Rate limits — `GET /v1/usage/rate-limits`

Live RPM / RPD / TPM counters vs. the effective limits (always fresh, never
cached). Only the `key` scope is populated; `org` / `team` / `member` are always
`null`.

Each metric carries `current`, the effective `limit`, `pct` (0–100), and
`resets_in_seconds`. RPM and TPM are per-minute windows; RPD resets at midnight
UTC. A limit the key does not explicitly set is shown as the system default with
`is_default_limit: true`. Use `pct` to draw a usage bar and warn as it nears 100.

## Balance — `GET /v1/balance`

Current credit balance, plus how much is **reserved** by in-flight operations
(realtime sessions, async background jobs, video generation) and therefore not
spendable.

If your key belongs to an organization, the organization’s shared balance is
returned — all members draw from one billing pool; a personal key with no org
gets its own balance. `available_usd` = `max(0, balance_usd − reserved_usd)`;
`reserved_breakdown` lists only the categories currently holding funds and sums
to `reserved_usd`.

## Notes

- **Money** is always a decimal string (never a float).
- **Caching:** API-key reads are computed fresh (never cached).
- **Errors** use the standard envelope —`401` (bad credential),`422` (bad
UUID / invalid filter),`429` (rate limited).

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/usage-monitoring-api
