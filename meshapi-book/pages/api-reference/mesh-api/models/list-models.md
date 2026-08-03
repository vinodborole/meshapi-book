---
type: Web Page
title: List Models | Mesh API Docs
description: List available models with per-user discounted pricing when applicable.
  Only models registered in the models table with isenabled=true are
resource: https://developers.meshapi.ai/api-reference/mesh-api/models/list-models
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# List Models

List available models with per-user discounted pricing when applicable.
Only models registered in the `models` table with is_enabled=true are
returned. Accepts either a dashboard JWT or an API key.
Includes pricing per 1 000 tokens and a convenience ``is_free`` flag.
If the caller has an active discount, discounted prices are included.
Response is cached for 5 minutes; admin writes invalidate the cache immediately.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

free

Filter: true = free models only, false = paid only, omit = all

type

Filter by model_type: text, embedding, image, audio, video

Allowed values:

provider

Filter by provider: amazon-bedrock, vertex, openai

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

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/models/list-models
