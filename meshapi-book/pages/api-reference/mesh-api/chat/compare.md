---
type: Web Page
title: Chat Compare | Mesh API Docs
resource: https://developers.meshapi.ai/api-reference/mesh-api/chat/compare
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Chat Compare

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

models

messages

model_overrides

comparison_model

comparison_instructions

temperature

max_tokens

stream

template

variables

skip_comparison

cache

### Response

Per-model results plus an optional synthesized comparison (JSON), or an SSE stream when stream=true

comparison_id

Unique ID for this comparison (`cmp_...`).

created

Unix timestamp (seconds) when the response was produced.

models

Models that were compared, in request order (deduped).

results

Per-model results, in `models` order.

total_latency_ms

End-to-end latency for the whole compare request, in milliseconds.

object

comparison

Synthesized evaluation of all responses from the comparison LLM. Null when `skip_comparison` is true or fewer than two models succeeded.

comparison_model

Model that produced `comparison`. Null when no synthesis ran.

comparison_usage

Token usage for the comparison LLM call.

comparison_fallback_used

True if the primary comparison model failed and a fallback produced the synthesis.

partial

True if at least one model in `results` returned an error.

skip_comparison

Echoes whether the comparison LLM step was skipped.

### Errors

401

Unauthorized Error

402

Payment Required Error

403

Forbidden Error

422

Unprocessable Entity Error

429

Too Many Requests Error

500

Internal Server Error

502

Bad Gateway Error

503

Service Unavailable Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/chat/compare
