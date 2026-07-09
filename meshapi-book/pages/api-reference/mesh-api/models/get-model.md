---
type: Web Page
title: Get Model | Mesh API Docs
description: Return a single enabled model by its model ID with per-user discounted
  pricing. Served from the same Redis cache as GET /v1/models — no additional DB queries
resource: https://developers.meshapi.ai/api-reference/mesh-api/models/get-model
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Get Model

Return a single enabled model by its model ID with per-user discounted pricing.

Served from the same Redis cache as GET /v1/models — no additional DB queries when the cache is warm. Discounts are applied per-user and not cached.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

model_id

### Response

Successful Response

id

name

context_length

is_free

pricing

supports_thinking

supports_completions_api

supports_responses_api

model_type

input_modalities

output_modalities

brand

provider

description

supports_realtime

supports_embeddings

supports_tools

supports_structured_output

supports_system_prompt

supports_batching

supports_background_response

supports_video_generation

supports_image_edit

supports_image_inpaint

supports_image_outpaint

supports_image_mix

supports_image_reframe

supports_image_upscale

supports_image_remove_background

supports_image_reference

context_window

standard_context_threshold

realtime_session_max_tokens

realtime_max_concurrent_per_owner

is_composite

composite_models

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/models/get-model
