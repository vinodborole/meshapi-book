---
type: Web Page
title: Realtime Audio | Mesh API Docs
description: WebSocket connection, auth, event-shape, session limits, and error codes
  for speech-to-speech.
resource: https://developers.meshapi.ai/debug/realtime-audio
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Realtime Audio

WebSocket connection, auth, event-shape, session limits, and error codes for speech-to-speech.

Realtime audio is a WebSocket gateway at `wss://api.meshapi.ai/v1/realtime`.
Most issues are connection, auth, or event-shape problems. See
Realtime Audio for the full reference.

## 426 Upgrade Required / can’t connect over HTTP

This endpoint exists **only** as a WebSocket upgrade. A plain `GET /v1/realtime`
over HTTP returns `426 Upgrade Required`. Also, TLS is mandatory — `ws://` is
rejected, use `wss://`.

## Auth fails, especially in the browser

Two methods: the `Sec-WebSocket-Protocol: openai-realtime, Bearer <rsk_...>`
subprotocol (preferred), or `?api_key=<rsk_...>` on the URL.

- Browsers can’t set request headers on a WebSocket and can’t reliably send the space-separated `Bearer`subprotocol — use the`?api_key=`query fallback.
- If both are present, the subprotocol wins.

## session.update is rejected

Realtime uses OpenAI’s **GA** event protocol. Use the GA shape
(`{"type":"realtime","output_modalities":[...],"audio":{...}}`). The older
**beta** shape (`modalities` / `voice` / `input_audio_format`) is rejected.

## Audio isn’t received / binary frames ignored

Audio frames are **base64 inside text events** (`input_audio_buffer.append` up,
`response.output_audio.delta` down). Binary WebSocket frames are **not
supported** — don’t send raw PCM as a binary frame.

## Session drops after a while

- **30-minute cap**— sessions are capped upstream; Mesh doesn’t extend it. Reconnect and resume app-level state for long agents.
- **60s idle timeout**— idle sockets (no client→server frames for 60s) are closed by ingress. Keep the audio buffer flowing or send a- `session.update`ping.

## Reading errors — use error.code, not the close code

Errors arrive as a JSON frame
(`{ "type": "error", "error": { "code", "message" }, "request_id" }`) just
before the socket closes. Branch on `error.code` — the close code is incidental.

Close codes seen: `1008` (policy/auth/quota), `1011` (server/upstream), `4402`
(quota/cap), `4000`–`4999` (forwarded from OpenAI).

## Session won’t open / billing surprises

- You need at least **$10 account balance**to open a realtime session.
- Sessions cut short (network drop, tab close) **still bill**for tokens already processed.
- There’s no in-stream cost message — query `GET /v1/usage`after the session for canonical numbers.

## Still stuck?

See the Mesh API error reference
or email **contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/realtime-audio
