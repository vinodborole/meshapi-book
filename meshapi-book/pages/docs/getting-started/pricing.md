---
type: Web Page
title: Pricing - Mesh API
description: Understand how usage is measured and charged.
resource: https://developers.meshapi.ai/docs/getting-started/pricing
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**pre-paid credit**model. You load funds into your account balance and we deduct fractions of a cent per token processed. There are no subscriptions or monthly minimums.

## How token pricing works

Usage is billed per token. Pricing is published per
**1 million tokens**(the industry-standard “per-1M” convention) and has two components:

Completion tokens are typically priced higher than prompt tokens. Different models have vastly different price points — a lightweight model may cost 50× less than a frontier reasoning model.
Token-priced models also publish per-1M rates for caching, batch, and other modalities where the provider supports them.

## Non-token pricing units

Not every model bills by tokens. Speech, image, and video models publish their rate in the unit they actually bill in — check the`pricing` object’s `pricing_unit` field (for example `per_second`, `per_1k_chars`, `per_image`, `per_video`, `per_hour`).
For these models `prompt_usd_per_1m` and `completion_usd_per_1m` are `null` (a per-token figure would be meaningless), and the real rate is published in:
For token-priced models (

`pricing_unit: per_1m_tokens`) the per-unit fields simply mirror the per-1M values.
## Flat per-call fees

A few capabilities are billed per call rather than per token.
[Web Search](/docs/capabilities/web-search)is a flat

**$0.005 per successful search**regardless of result count — failed or empty searches are free. Budget these by call count, not token volume.

## Free vs. paid models

**Free models**(

`is_free: true`) cost $0 for both prompt and completion tokens. They’re ideal for testing, prototyping, and low-stakes classification tasks.
**Paid models**charge variable rates based on the model’s computational cost. Before using paid models, you need a positive credit balance.

You can filter for free models via the Dashboard → 

**Models**tab or by calling`GET /v1/models` and checking `is_free: true`.
## Pre-paid balance

1. Add credits in the **Billing** section of the[Dashboard](https://app.meshapi.ai)
2. Credits are debited in real time as inference completes
3. When your balance hits zero, paid model requests return `HTTP 402 Payment Required` with error code`spend_limit_exceeded`
4. Free models continue working regardless of balance

Billing settles 

*after*a response is produced, because the true token count isn’t known until then. That means your balance can dip slightly below zero on the last request — it settles on your next top-up.
### Auto-recharge

Turn on auto-recharge in
**Dashboard → Billing**so a long job never dies mid-run with a

`402`. Each recharge can be $5–$100, and you can have one active auto-recharge setup per account. Check status with `GET /v1/auto-recharge`.
## Spend caps

Every API key can have an independent
**spend cap**(a maximum USD amount). Once a key’s spend reaches the cap, further paid requests from that key are blocked until you raise or reset the cap. This is your primary protection against runaway costs — always set a cap on every key.

## Checking your balance

`GET /v1/balance` accepts **either**a dashboard session token (JWT) or an

`rsk_` API key.
`reserved_usd` is the portion held by in-flight operations — realtime sessions, background jobs, video generation — and `available_usd` is `max(0, balance_usd − reserved_usd)`, the amount you can actually spend. `reserved_breakdown` lists only the categories currently holding funds and sums to `reserved_usd`. `message` is non-null only when an error condition is reported.
With an org context the **org owner’s**balance is returned — all members draw from one billing pool. See the

[Usage & Monitoring API](/docs/reference/usage-api)for the full monitoring surface.

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/pricing
