---
type: Web Page
title: Create Speech | Mesh API Docs
description: Convert text to speech. Provider is resolved automatically from the model
  name via the modelpricing
resource: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-speech
timestamp: '2026-07-20T09:25:48.943332+00:00'
---

# Create Speech

Convert text to speech.

Provider is resolved automatically from the model name via the model_pricing
table — no hardcoded provider branching. Streaming is used when the provider
adapter supports it and `stream=true` (the default).

The `voice` field is required for ElevenLabs models. Sarvam models use
`speaker` instead.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

input

model

voice

stream

response_format

language_code

voice_settings

pronunciation_dictionary_locators

seed

previous_text

next_text

previous_request_ids

next_request_ids

apply_text_normalization

apply_language_text_normalization

use_pvc_as_ivc

enable_logging

optimize_streaming_latency

speaker

target_language_code

pitch

pace

loudness

speech_sample_rate

enable_preprocessing

### Response

Successful Response

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-speech
