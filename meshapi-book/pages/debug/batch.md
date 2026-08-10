---
type: Web Page
title: Batch API - Mesh API
description: Fix batch job submission errors, stuck jobs, and result download problems.
resource: https://developers.meshapi.ai/debug/batch
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

[Batch API](/docs/capabilities/batch-api)for the full guide.

## 400 mixed_models — all requests must use one model

A batch
**cannot mix models**— every item in

`requests` must target the same
model. Split different models into separate batches.
## 429 batch_limit_exceeded

You can have at most
**10 batches in a non-terminal state**at once. Wait for in-flight batches to reach a terminal status (

`completed`, `failed`,
`cancelled`, `expired`) before creating more.
## Results are out of order / I can’t match them

Output order is
**not guaranteed**. Match each result to its request by the

`custom_id` you supplied (it’s echoed back on the matching result).
## Batch completed but some items failed

A`completed` batch can still contain failed items. Check two levels:
- **Batch level** —`status` and`request_counts` (`total` /`completed` /`failed` ).
- **Item level** — each result’s`response.status_code` and`error` field.

## It’s taking too long / expired

Batches are optimized for throughput, not interactive latency. They run within the`completion_window` you set (e.g. `"24h"`); if a batch doesn’t finish in
time it moves to `expired`. Don’t use batches for low-latency calls — use
`POST /v1/chat/completions` directly.
## Managing in-flight batches

- `GET /v1/batches` lists recent batches.
- `POST /v1/batches/{batch_id}/cancel` cancels one (moves through`cancelling` →`cancelled` ).
- Statuses progress `validating → in_progress → finalizing → completed` .

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/batch
