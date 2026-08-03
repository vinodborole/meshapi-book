---
type: Web Page
title: Audio (TTS & STT) | Mesh API Docs
description: Text-to-speech, speech-to-text, and voice management with the Python
  SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/audio-tts-stt
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Audio (TTS & STT)

Audio (TTS & STT)

# Audio

## Text-to-Speech

`client.audio.synthesize` sends `POST /v1/audio/speech` and returns raw audio bytes.

### Async

### `SpeechParams` fields

## Speech-to-Text (Transcription)

`client.audio.transcribe` sends `POST /v1/audio/transcriptions` as a multipart upload and returns a `TranscriptionResponse`.

### `TranscriptionParams` key fields

## Translation

`client.audio.translate` sends `POST /v1/audio/transcriptions/translate` and returns the audio transcribed and translated to English.

## Translation (to English)

`client.audio.audio_translate` sends `POST /v1/audio/translations` and returns the audio translated directly to English. This is a distinct endpoint from the transcribe-and-translate helper above.

### Async

### `AudioTranslationsParams` fields

The response `.text` field contains the English translation.

## List Voices

`client.audio.list_voices` sends `GET /v1/audio/voices`.

### `ListVoicesParams` fields

## Get Voice

`client.audio.get_voice` sends `GET /v1/audio/voices/{voice_id}`.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/audio-tts-stt
