---
type: Web Page
title: Speech-to-Text | Mesh API Docs
description: Transcription request shape, input sources, translation support, and
  realtime WebSocket frames.
resource: https://developers.meshapi.ai/debug/speech-to-text
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Speech-to-Text

Speech-to-Text

Transcription runs over `POST /v1/audio/transcriptions` (REST) and
`WS /v1/audio/transcriptions/realtime` (streaming). See
[Speech-to-Text](/speech-to-text) for the full reference.

## 422 — the endpoint expects multipart/form-data, not JSON

`POST /v1/audio/transcriptions` uses **`multipart/form-data`**, not a JSON body.
Send fields as form parts (`-F` in curl, `files=`/`data=` in httpx). Posting
JSON is the most common cause of a 422 here.

## No audio provided

You must supply exactly one input source: a `file` upload, a `source_url`
(public URL), or a `cloud_storage_url` (S3/GCS). Omitting all three fails.

## 422 on transcribe-and-translate

`POST /v1/audio/transcriptions/translate` requires a model that **supports
translation** (defaults to `sarvam/saaras:v2`). If the selected model can’t
translate, you get a `422`. Confirm capabilities via `GET /v1/models`.

## Realtime WebSocket won’t connect or auth fails

Pass the key one of three ways: `Sec-WebSocket-Protocol: Bearer rsk_...`,
`?api_key=rsk_...`, or `?token=rsk_...`. In browsers (no custom headers), use
the query parameter.

## Realtime transcripts are empty or garbled

- `audio_format` must match what you actually send (default`pcm_16000` ; the standard protocol expects**PCM `s16le`, 16 kHz, mono** ).
- With `commit_strategy: manual` you must send`input_audio_buffer.commit` (or ElevenLabs`commit: true` ) to get a final transcript;`auto` uses VAD.
- **ElevenLabs realtime** only forwards`input_audio_chunk` frames — any other message type is silently dropped.

## WebSocket closed with an error

Realtime STT sends a JSON `error` frame and then closes with code `1000`.
Read the `error` frame for the reason (auth or upstream). REST errors use
standard codes: `401`, `422`, `429`, `402`.

## Still stuck?

See the [Mesh API error reference](/debug/mesh-api#error-code-reference)
or email **[contact@meshapi.ai](mailto:contact@meshapi.ai)**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/speech-to-text
