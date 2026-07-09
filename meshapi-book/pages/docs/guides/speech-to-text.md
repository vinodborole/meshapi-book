---
type: Web Page
title: Speech-to-Text | Mesh API Docs
description: Text-to-speech, speech-to-text, voice management, and real-time streaming
  audio APIs.
resource: https://developers.meshapi.ai/docs/guides/speech-to-text
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

Speech-to-Text

Speech-to-Text

# Audio Generation

Mesh API provides a full suite of audio endpoints — convert text to speech, transcribe audio files, stream TTS/STT in real time, and browse available voices — all through a single API key.

All endpoints share the same base URL: `https://api.meshapi.ai/v1/audio`

**Auth:** `Authorization: Bearer rsk_<your-key>` on all REST requests. WebSocket endpoints accept the key via `Sec-WebSocket-Protocol: Bearer <rsk_...>` or `?api_key=<rsk_...>`.

## Speech-to-Text

`POST /v1/audio/transcriptions`

Transcribe an audio file. The provider is resolved automatically from the model name. You can supply audio as a file upload, a public URL, or a cloud storage URL.

This endpoint uses `multipart/form-data` — not JSON.

Multiple STT providers are supported — e.g. `elevenlabs/scribe_v1` (default) and `elevenlabs/scribe_v2`, and `sarvam/saaras:v2`. To list transcription-capable models, use the search endpoint filtered by modality: `GET /v1/models/search?input_modality=audio&output_modality=text`.

### Form fields

### Response

### Examples

###### curl (file upload)

###### curl (URL)

###### Python

###### Node.js

## Transcribe and Translate

`POST /v1/audio/transcriptions/translate`

Transcribe audio and translate the result directly to English in a single step. Uses Sarvam models by default.

This endpoint uses `multipart/form-data`.

### Form fields

### Response

### Example

If the selected model doesn’t support translation, the API returns a `422` error. Check `GET /v1/models` to confirm a model’s capabilities.

## WebSocket Real-Time STT

`WS /v1/audio/transcriptions/realtime`

Transcribe audio in real time. Send raw audio chunks as they are captured (e.g. from a microphone) and receive partial and final transcripts as they are produced.

The wire protocol is selected automatically from the model you pass. Standard streaming models (e.g. `openai/whisper-large-v3`) use an OpenAI-compatible frame protocol. ElevenLabs models (`elevenlabs/scribe_v2_realtime`) use ElevenLabs’ Scribe v2 realtime frames — see the ElevenLabs models subsection below.

### Authentication

Pass your Mesh API key in one of these ways:

- `Sec-WebSocket-Protocol: Bearer rsk_...`header
- `?api_key=rsk_...`query parameter
- `?token=rsk_...`query parameter

### Query parameters

**Supported audio formats:** `pcm_8000`, `pcm_16000`, `pcm_22050`, `pcm_24000`, `pcm_44100`, `pcm_48000`, `ulaw_8000`

### Standard streaming models

Standard streaming models — e.g. `openai/whisper-large-v3` — use an OpenAI-compatible frame protocol.

**Client → server (JSON frames)**

**Server → client (JSON frames)**

### Example

### ElevenLabs models

ElevenLabs models (`elevenlabs/scribe_v2_realtime`) use ElevenLabs’ Scribe v2 realtime frames.

**Client → server (JSON frames)**

Send `input_audio_chunk` frames with base64-encoded audio. This is the only message type the server forwards upstream — any other message type is silently dropped.

Set `"commit": true` to trigger a VAD commit when using `commit_strategy: manual`.

**Server → client (JSON frames)**

## Voice Management

### List voices

`GET /v1/audio/voices`

Returns a unified voice catalog spanning every TTS model brand. The list can be
large, so **filter by  brand or model** to narrow it to the voices you can
actually use with the model you plan to call.

Each voice is returned with `voice_id`, `name`, `brand`, `provider`, `model`,
`language`, `gender`, and `preview_url`. The response also carries `count` and a
`truncated` flag (`true` when the list was capped before the full set was fetched).

Pass a voice’s `voice_id` as the `voice` field on `POST /v1/audio/speech` (or the
`{voice_id}` path segment on the streaming endpoint) — and use the same `model`
the voice is listed under. Filtering by `brand`/`model` keeps you from picking a
voice that isn’t valid for the model you call.

### Get a single voice

`GET /v1/audio/voices/{voice_id}`

Fetch metadata for a specific voice by its ElevenLabs voice ID.

## Error handling

All endpoints use standard HTTP status codes. Common cases:

WebSocket sessions send a JSON `error` frame and then close with code `1000` before disconnecting.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/speech-to-text
