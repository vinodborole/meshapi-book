---
type: Web Page
title: Realtime Audio | Mesh API Docs
description: Bidirectional speech-to-speech WebSocket sessions with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/realtime-audio
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Realtime Audio

# Realtime Audio

`client.Realtime` opens a bidirectional WebSocket session to `wss://api.meshapi.ai/v1/realtime`. The wire format is identical to OpenAI’s Realtime API — every event you send and receive is shaped exactly as upstream documents it.

## Connect and close

## Configure the session

## Send audio

## RealtimeMessage

Every frame from the server is a `meshapi.RealtimeMessage`. Exactly one of the fields below is non-zero per message:

Output audio arrives in-band as base64 inside `response.output_audio.delta` events; the SDK decodes it into `Audio`, so those frames populate **both** `Audio` and `Event`. Check `len(msg.Audio) > 0` before switching on `Event["type"]`.

## Receive frames

`Receive` blocks until the next frame arrives. Context cancellation unblocks it immediately:

## Events channel (concurrent pump)

For concurrent send/receive, use `Events` to pump frames into a channel:

## Full voice agent example

## Error handling

Server errors arrive as a `*meshapi.RealtimeError` from `Receive` or on the `errCh` returned by `Events`:

## Supported models

Consult `GET /v1/models` for the current set of realtime-capable models.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/realtime-audio
