---
type: Web Page
title: Text-to-Speech - Mesh API
description: Fix speech synthesis errors, voice lookup failures, and audio format
  problems.
resource: https://developers.meshapi.ai/debug/text-to-speech
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/audio/speech` (REST) and
`WS /v1/audio/speech/stream/{voice_id}` (streaming). See
[Text-to-Speech](/docs/capabilities/text-to-speech)for the full reference.

## The response won’t parse as JSON

`POST /v1/audio/speech` returns **raw audio bytes**, not JSON. The

`Content-Type` matches the requested format (e.g. `audio/mpeg`, `audio/wav`).
Write the body straight to a file — don’t call `.json()` on it.
## Voice errors / wrong or missing voice

- `voice` is**required for ElevenLabs models** and must be a valid ID for that model’s brand.
- A voice from one brand won’t work with a model from another. Browse valid IDs with `GET /v1/audio/voices` and**filter by `brand` or `model`** so you only pick voices usable with the model you call.
- Sarvam models use `speaker` (default`anushka` ), not`voice` .

## response_format rejected

Not every format is valid in every mode.`wav_*` formats are **non-streaming only**— request them with

`stream: false`. Streaming supports `mp3_*`,
`pcm_*`, `ulaw_8000`, `alaw_8000`, and `opus_*`.
## WebSocket streaming produces no audio

The frame protocol depends on the model family:
- **Standard models** (Kokoro, Cartesia, …) — send`input_text_buffer.append` then`input_text_buffer.commit` ; audio arrives as`conversation.item.audio_output.delta` (base64).
- **ElevenLabs models** — the**first** frame must be`initializeConnection` (`{ "text": " " }` ), then`sendText` , then`closeConnection` (`{ "text": "" }` ); audio arrives as`AudioOutput` (base64).

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/text-to-speech
