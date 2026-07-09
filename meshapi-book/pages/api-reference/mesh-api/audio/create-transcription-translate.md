---
type: Web Page
title: Create Transcription Translate | Mesh API Docs
description: Transcribe audio and translate it to English. Legacy alias for POST /v1/audio/translations
  — prefer that
resource: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-transcription-translate
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Transcription Translate

Transcribe audio and translate it to English.

**Legacy alias** for `POST /v1/audio/translations` — prefer that
OpenAI-standard path. Behaviour is identical; retained so existing callers
keep working.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects a multipart form containing a file.

model

file

prompt

### Response

Successful Response

text

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/audio/create-transcription-translate
