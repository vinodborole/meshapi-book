---
type: Web Page
title: Create Translation | Mesh API Docs
description: Translate speech in any language to English text. OpenAI-compatible POST
  /v1/audio/translations — the path the OpenAI SDK's
resource: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-translation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Translation

Translate speech in any language to English text.
OpenAI-compatible `POST /v1/audio/translations` — the path the OpenAI SDK's
`client.audio.translations.create()` calls. Provider is resolved from the
model name via model_pricing; the adapter must implement
`speech_to_text_translate()` (providers that don't → 422).
`response_format`: `json` (default) → `{"text": "..."}`; `text` → the raw
transcript as `text/plain`; `verbose_json` → the per-word structure. All in
English.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects a multipart form containing a file.

model

file

prompt

response_format

temperature

### Response

Successful Response

text

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-translation
