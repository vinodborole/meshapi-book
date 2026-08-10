---
type: Web Page
title: Audio - Mesh API
description: Send audio into chat completions and request audio output from supported
  models.
resource: https://developers.meshapi.ai/docs/capabilities/audio
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/chat/completions`.
Use this page for:
- audio input with `input_audio`
- audio output with `modalities` and`audio`

## Audio input

Send audio as a content part inside a chat message.
- curl
- Node.js SDK
- Python SDK
- Go SDK
- Java SDK

## Audio output

Request text and audio together when the model supports audio output.`modalities` and `audio`.
## Translate audio to English

`POST /v1/audio/translations` accepts audio in any language and returns the speech **translated to English**. It returns the same

`TranscriptionResponse` (with a `.text` field) as transcription.
This is a distinct endpoint from the transcribe-and-translate helper at 

`POST /v1/audio/transcriptions/translate`. Check `GET /v1/models` for models that support translation — `model` is required.
- Python SDK
- Node.js SDK
- Go SDK

`prompt` (context hint for the model), `response_format` (`json`, `text`, or `verbose_json`), and `temperature` (0–2).
## SDK coverage

- Node: `client.chat.completions.create(...)`
- Python: `client.chat.completions.create(...)`
- Go: `client.Chat.Completions.Create(...)`
- Java: `client.chat().completions().create(...)`

## Notes

- Audio payloads are base64 encoded in the request body.
- Check `GET /v1/models` to find models that accept or produce audio.
- Keep payload sizes reasonable, especially for browser-based clients.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/audio
