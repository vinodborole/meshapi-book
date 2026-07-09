---
type: Web Page
title: Compare | Mesh API Docs
description: Partial results, model limits, billing, and the two streaming modes.
resource: https://developers.meshapi.ai/debug/compare
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

# Compare

`POST /v1/chat/compare` runs one prompt across multiple models concurrently.
See [Compare](/compare) for the full guide.

## Some models are missing from the result (partial: true)

Model calls are isolated — if one fails or hits the **120s hard timeout**, the
others continue and the response carries `partial: true`. Check each result’s
`error` / `error_code`; a partial response is expected behavior, not a failure
of the whole call.

## Too many / duplicate models

`models` accepts **1–10** entries. Duplicates are removed automatically, so
listing the same model twice won’t run it twice.

## Billing is higher than expected

Compare bills **N+1** usage events: one per fan-out model **plus** the
synthesis/comparison call. (For rate limits, though, the whole comparison
counts as a **single** request against RPM/RPD.) Set `skip_comparison: true` to
drop the comparison call if you only need raw outputs.

## Streaming behaves differently than chat

There are two modes:

- **With comparison**(default) — fan-out models are- **non-streaming**; only the final comparison text streams token-by-token (- `comparison_chunk`).
- `skip_comparison: true`- `model_chunk`/- `model_stream_done`).

Watch for `comparison_fallback_used` in the `done` event if the comparison
model had to fall back.

## Still stuck?

See the [Mesh API error reference](/debug/mesh-api#error-code-reference)
or email ** contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/compare
