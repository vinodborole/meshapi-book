---
type: Web Page
title: Create Transcription | Mesh API Docs
description: Transcribe an audio file to text. Provider is resolved automatically
  from the model name via modelpricing.
resource: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-transcription
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Transcription

Transcribe an audio file to text.
Provider is resolved automatically from the model name via model_pricing.
`file` is optional for providers that accept `source_url` or `cloud_storage_url`.
`additional_formats` is a JSON-encoded string (ElevenLabs-specific, ignored by others).
Returns `{"text": "..."}` by default, or per-word structure (`words[]` with each
token's `type`) when `response_format=verbose_json`.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

enable_logging

### Request

This endpoint expects a multipart form containing an optional file.

model

file

response_format

language_code

tag_audio_events

num_speakers

timestamps_granularity

diarize

diarization_threshold

additional_formats

file_format

cloud_storage_url

source_url

webhook

webhook_id

temperature

seed

use_multi_channel

webhook_metadata

entity_detection

no_verbatim

detect_speaker_roles

entity_redaction

entity_redaction_mode

keyterms

with_timestamps

debug_mode

### Response

Successful Response

text

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-transcription
