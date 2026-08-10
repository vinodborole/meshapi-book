---
type: Web Page
title: Realtime Audio - Mesh API
description: Connect to a bidirectional WebSocket session for real-time audio and
  text.
resource: https://developers.meshapi.ai/sdk/realtime
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**Python**requires

`pip install 'meshapi[realtime]'` (adds `websockets>=12.0`).
**Node.js**on Node 18–21 requires

`npm install ws`. Node 22+ has WebSocket built in.
**Protocol.**The realtime API uses OpenAI’s GA event shape: configure the session with

`session.type: "realtime"`, `output_modalities`, and an `audio` object (see
below). Input audio is sent as base64 (`send_audio` handles this for you), and
**output audio arrives as**— the SDK decodes these into

`response.output_audio.delta` events`msg.audio` so you can play them directly. Audio is 24 kHz mono PCM16.
## Connect and configure

- Python
- Node.js
- Go

## Receive frames

The server sends a`session.created` event immediately after the WebSocket handshake completes.
- Python
- Node.js
- Go

## Send audio

Pass raw 16-bit PCM audio (24kHz mono) to`send_audio` / `sendAudio` / `SendAudio`.
- Python
- Node.js
- Go

## Error handling

Errors in the WebSocket session are surfaced as`RealtimeError` (Python/Node.js) or `*meshapi.MeshAPIError` (Go).
- Python
- Node.js
- Go

## Supported models

Consult 

`GET /v1/models` for the current set of realtime-capable models — the
catalog is live and this table is a snapshot.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/realtime
