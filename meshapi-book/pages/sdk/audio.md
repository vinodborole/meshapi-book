---
type: Web Page
title: Audio - Mesh API
description: Text-to-speech synthesis, speech-to-text transcription, and listing available
  voices.
resource: https://developers.meshapi.ai/sdk/audio
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Text to speech

`audio.synthesize` returns raw audio bytes.
- Python
- Node.js
- Go

## Speech to text

`audio.transcribe` accepts raw audio bytes plus a filename hint for format detection.
**Argument order differs by language:**

- Python: `transcribe(bytes, TranscriptionParams, filename=)`
- Node.js: `transcribe(audio, { model }, { filename })`
- Go: `Transcribe(ctx, bytes, filename, TranscriptionParams)`

- Python
- Node.js
- Go

## Translate audio to English

`POST /v1/audio/translations` transcribes audio in any language and returns the text **translated to English**. It returns the same

`TranscriptionResponse` (with a `.text` field) as transcription.
This is a distinct endpoint from the transcribe-and-translate helper (

`POST /v1/audio/transcriptions/translate`). Use a speech model that supports translation — see the [Models](/sdk/models)list.`model` is required.
- Python
- Node.js
- Go

`prompt` (context hint), `response_format` (`json`, `text`, or `verbose_json`), and `temperature` (0–2).
## List voices

- Python
- Node.js
- Go

### `ListVoicesParams` fields

## Get a voice

Fetch a single voice by ID —`GET /v1/audio/voices/{voice_id}`.
- Python
- Node.js
- Go

# Citations

1. Source page: https://developers.meshapi.ai/sdk/audio
