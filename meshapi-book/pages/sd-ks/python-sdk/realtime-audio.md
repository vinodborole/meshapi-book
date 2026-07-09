---
type: Web Page
title: Realtime Audio | Mesh API Docs
description: Bidirectional speech-to-speech WebSocket sessions with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/realtime-audio
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Realtime Audio

# Realtime Audio

`client.realtime` opens a bidirectional WebSocket session to `wss://api.meshapi.ai/v1/realtime`. The wire format is identical to OpenAI’s Realtime API.

Requires `websockets>=12.0`. Install with `pip install 'meshapi[realtime]'`.

**Protocol.** Configure the session with the GA event shape: `session.type: "realtime"`,
`output_modalities`, and an `audio` object (below). Input audio is sent as base64 —
`send_audio()` handles that — and **output audio arrives as  response.output_audio.delta
events**, which the SDK decodes into 

`msg.audio`. Audio is 24 kHz mono PCM16.## Connect and close (sync)

## Connect and close (async)

## Configure the session

## Send audio

## Receive frames

## Iterate over frames

## Async iteration

## Error handling

## Full voice agent example

## Supported models

Consult `GET /v1/models` for the current set of realtime-capable models.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/realtime-audio
