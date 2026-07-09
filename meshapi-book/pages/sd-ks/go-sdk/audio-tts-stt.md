---
type: Web Page
title: Audio (TTS & STT) | Mesh API Docs
description: Text-to-speech, speech-to-text, and voice management with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/audio-tts-stt
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

Audio (TTS & STT)

Audio (TTS & STT)

# Audio

## Text-to-Speech

`client.Audio.Synthesize` sends `POST /v1/audio/speech` and returns `[]byte` of raw audio.

`SpeechParams` fields

## Speech-to-Text (Transcription)

`client.Audio.Transcribe` sends `POST /v1/audio/transcriptions` as a multipart upload and returns `*TranscriptionResponse`.

With keyterms (sent as repeated form fields):

`TranscriptionParams` key fields

## Transcribe and Translate

`client.Audio.Translate` sends `POST /v1/audio/transcriptions/translate` — it transcribes audio and translates the result to English in one step.

## Translation (to English)

`client.Audio.Translations` sends `POST /v1/audio/translations` — a dedicated translation endpoint that translates audio directly to English. Pick a translation-capable model from the [Models list](/sdks/go/models).

`AudioTranslationParams` fields

The response `Text` field contains the English translation.

## List Voices

`client.Audio.ListVoices` sends `GET /v1/audio/voices`.

`ListVoicesParams` fields

## Get Voice

`client.Audio.GetVoice` sends `GET /v1/audio/voices/{voice_id}`.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/audio-tts-stt
