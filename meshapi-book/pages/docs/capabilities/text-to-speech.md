---
type: Web Page
title: Text to Speech - Mesh API
description: Text-to-speech, speech-to-text, voice management, and real-time streaming
  audio APIs.
resource: https://developers.meshapi.ai/docs/capabilities/text-to-speech
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

# Audio Generation

Mesh API provides a full suite of audio endpoints — convert text to speech, transcribe audio files, stream TTS/STT in real time, and browse available voices — all through a single API key. All endpoints share the same base URL:`https://api.meshapi.ai/v1/audio`
**Auth:**

`Authorization: Bearer rsk_<your-key>` on all REST requests. WebSocket endpoints accept the key via `Sec-WebSocket-Protocol: Bearer <rsk_...>` or `?api_key=<rsk_...>`.
## Text-to-Speech

`POST /v1/audio/speech`
Convert a text string into audio. The brand behind a model (ElevenLabs, Sarvam, etc.) is selected automatically based on the model you pass — models like `hexgrad/kokoro-82m` and `cartesia/sonic-2` are also available. Streaming is enabled by default.
### Request body

### Supported output formats

**Streaming (**

`stream: true`):`mp3_22050_32`, `mp3_24000_48`, `mp3_44100_32/64/96/128/192`, `pcm_8000/16000/22050/24000/32000/44100/48000`, `ulaw_8000`, `alaw_8000`, `opus_48000_32/64/96/128/192`
**Non-streaming (**All of the above, plus

`stream: false`):`wav_8000/16000/22050/24000/32000/44100/48000`
### Response

The response body is raw audio bytes with the`Content-Type` matching the requested format (e.g. `audio/mpeg`, `audio/wav`).
### Examples

- curl (streaming)
- curl (non-streaming WAV)
- Python
- Node.js

## WebSocket TTS Streaming

`WS /v1/audio/speech/stream/{voice_id}`
Stream text-to-speech in real time. You send text chunks as they become available (e.g. as an LLM streams tokens), and receive audio back chunk by chunk — minimising latency compared to the REST endpoint. The streaming frame protocol depends on the model family. The `voice_id` is part of the URL path.
- **Standard streaming models** — e.g.`hexgrad/kokoro-82m` ,`cartesia/sonic-2` ,`cartesia/sonic-3` ,`canopylabs/orpheus-3b-0.1-ft` — use the text-buffer protocol.
- **ElevenLabs models** (`elevenlabs/*` ) use ElevenLabs’ native stream-input frames.

### Authentication

Pass your Mesh API key in one of two ways:
- `Sec-WebSocket-Protocol: Bearer rsk_...` header
- `?api_key=rsk_...` query parameter

### Query parameters

ElevenLabs models additionally accept 

`enable_logging` (`true`/`false` logging opt-out), `enable_ssml_parsing` (enable SSML in the input text), `inactivity_timeout` (seconds of inactivity before the session closes, 1–180), `sync_alignment` (return word-level alignment data with each audio chunk), `auto_mode` (optimise for low-latency, fully-automated generation), `apply_text_normalization` (`auto`, `on`, or `off`), and `seed` (reproducible seed, 0–4294967295).
### Message protocol — standard streaming models

**Client → server (JSON frames)**

**Server → client (JSON frames)**

#### Example

### Message protocol — ElevenLabs models

**Client → server (JSON frames)**

**Server → client (JSON frames)**

Any credential fields (

`xi-api-key`, `authorization`, `api_key`) in client frames are stripped before forwarding upstream — your upstream credentials are never exposed to the client.
#### Example

### Supported output formats

**ElevenLabs models:**

`mp3_22050_32`, `mp3_44100_32/64/96/128/192`, `pcm_16000/22050/24000/44100`, `ulaw_8000`
**Standard streaming models:**

`pcm`, `mp3`, `wav`, `opus`, `aac`, `flac`, each with an optional sample rate (e.g. `pcm_24000`).
## Together AI voices

Together-hosted TTS models (Kokoro, Orpheus, Cartesia Sonic) don’t use the ElevenLabs voice catalogue — each model has its own set of voices passed in the same`voice` field. The `/v1/audio/voices` endpoint lists ElevenLabs voices only; use the references below for Together models.
### Kokoro — `hexgrad/kokoro-82m`

Pass a voice name such as `af_bella`. Kokoro also supports **voice mixing**: join two or more voices with

`+` (e.g. `af_bella+af_heart`), with optional per-voice weights in parentheses (e.g. `af_bella(2)+af_heart(1)`).
A selection of the 50+ available voices (prefix key: `a`=American, `b`=British, `j`=Japanese, `z`=Chinese, `e`=Spanish, `f`=French, `h`=Hindi, `i`=Italian, `p`=Portuguese; `f`=female, `m`=male):
### Orpheus — `canopylabs/orpheus-3b-0.1-ft`

One of: `tara`, `leah`, `jess`, `leo`, `dan`, `mia`, `zac`, `zoe`.
### Cartesia Sonic — `cartesia/sonic-2`, `cartesia/sonic-3`

Pass the voice **display name**(not a voice ID). 130+ voices are available; a selection of English voices:

**Streaming with Together models.**When

`stream: true`, Together TTS models only emit raw PCM — set `response_format` to a `pcm_*` value (e.g. `pcm_16000`, `pcm_24000`). For non-streaming requests (`stream: false`) you can also use `mp3_*` or `wav_*`.
## Available voices

List available voices (ElevenLabs voices):
**Response:**

## Available models

Together TTS voices are passed in the 

`voice` field but follow each model’s own naming — see

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/text-to-speech
