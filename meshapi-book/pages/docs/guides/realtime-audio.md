---
type: Web Page
title: Realtime Audio | Mesh API Docs
description: Bi-directional speech-to-speech over WebSocket using OpenAI's Realtime
  API through Mesh.
resource: https://developers.meshapi.ai/docs/guides/realtime-audio
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Realtime Audio

## Overview

`wss://api.meshapi.ai/v1/realtime` is a WebSocket gateway for low-latency
realtime audio. It supports two modes: bidirectional speech-to-speech via
OpenAI’s Realtime API, and realtime speech-to-text via ElevenLabs Scribe.

- Same auth surface as the rest of Mesh — your `rsk_...` data-plane token.
- Wire format is **identical** to OpenAI’s Realtime API. Mesh passes JSON event
bodies through verbatim in both directions, so any client written against
the upstream spec works against Mesh by switching the WebSocket URL.
- Billed on usage, metered at session close.

It’s intended for voice agents, live transcription with response, and any half-duplex / full-duplex audio UX where round-trip latency matters.

## Quickstart

Open a WebSocket to `wss://api.meshapi.ai/v1/realtime` with a `model` query
parameter, send a `session.update`, then stream audio chunks via
`input_audio_buffer.append` events.

###### Browser (raw WebSocket)

###### curl-style probe

## Authentication

Two methods are supported. Both require TLS — `ws://` is rejected.

**1. Subprotocol header (preferred).** Send your key in the `Sec-WebSocket-Protocol`
header alongside the literal protocol marker:

This matches OpenAI’s upstream auth and keeps the key out of URL logs.

**2. Query string fallback.** For browsers and tools that can’t customize the
subprotocol cleanly, append `?api_key=<YOUR_RSK_KEY>` to the URL. The key is
redacted from server access logs but **will** appear in client-side history,
so prefer the subprotocol method anywhere you control the runtime.

If both are present, the subprotocol wins.

## Message protocol

Mesh is a **transparent proxy** for OpenAI’s Realtime API. Every event you
send and receive is shaped exactly as upstream documents it — Mesh does not
rewrite event types, field names, or payloads.

See the [OpenAI Realtime API reference](https://platform.openai.com/docs/api-reference/realtime)
for the full event catalog. The most common types you’ll exchange:

Realtime uses OpenAI’s **GA** event protocol. `session.update` must use the GA
shape (`{"type":"realtime","output_modalities":[...],"audio":{...}}`) — the
older beta shape (`modalities`/`voice`/`input_audio_format`) is rejected.
Audio frames are base64 inside text events; binary WebSocket frames are not
supported.

## Supported models

The `model` query parameter is required. Realtime supports two provider modes:

Consult `GET /v1/models` for the current set of realtime-capable models.

## Pricing

**MeshAPI charges OpenAI’s exact rates for realtime models — zero markup, no
surprises.** Rates are subject to OpenAI’s pricing changes; consult `/v1/models`
for the canonical current values.

Per-session usage and cost lands in your existing `/v1/usage` history and the
dashboard. There is no in-stream cost message on the WebSocket — query
`/v1/usage` after the session completes.

### Pre-flight pricing

The `/v1/models` response indicates which models support realtime and
includes per-million-token rates for text, audio, and cached audio inputs
and outputs. Use these to display estimates in your UI before opening a
session — no need to hard-code rates in the client.

## Billing

- **Account balance required.** You need at least 10 USD account balance to
open a realtime session. If your account balance is exhausted during a
session, the connection is closed with an`insufficient_quota` error.
Top up to reconnect. Partial responses up to the point of disconnect
are still billed.
- **Session token caps.** Sessions configured with a max-token cap close
with a`session_token_cap_exceeded` error once the cap is reached.

Usage events are written to your account’s usage log at session close,
accessible via `GET /v1/usage` and the dashboard. Sessions that get cut
short (network drop, browser tab close) still bill for the tokens that
were processed. Query `/v1/usage` for canonical numbers.

## Limits and known caveats

- **Session length.** Sessions are capped at 30 minutes by upstream; Mesh
doesn’t extend this. Long-running agents should reconnect and resume
application-level state.
- **Ingress timeout.** Idle sockets (no client→server frames for 60s) are
closed by the L7 ingress. Send a`session.update` ping or keep the audio
buffer flowing.
- **Browsers / Safari.** Browsers can’t set request headers on a WebSocket and
can’t reliably send the space-separated`Bearer <key>` subprotocol token. In
the browser, authenticate with the`?api_key=<rsk_...>` query fallback.
- **No HTTP fallback.** This endpoint only exists as a WebSocket upgrade.`GET /v1/realtime` over plain HTTP returns`426 Upgrade Required` .

## Errors

Realtime errors are delivered as a JSON frame
(`{ "type": "error", "error": { "code", "message" }, "request_id": "..." }`)
immediately before the WebSocket is closed. Use `error.code` for
programmatic handling — it is the stable, semantic identifier. The
WebSocket close code that follows is incidental.

### `error.code` reference

In-band errors during a live session arrive as a regular `error` event
with the OpenAI-shaped envelope shown above and are not accompanied by a
socket close.

### WebSocket close codes (incidental)

These close codes accompany the JSON error frame on session-terminating
errors. Check `error.code` for the semantic reason; the close code below
is informational only.

## Next steps

- Review the [Authentication guide](/authentication) for key rotation and scoping.
- See the [API reference entry](/api-reference) for the OpenAPI stub.
- Watch the upstream [OpenAI Realtime API reference](https://platform.openai.com/docs/api-reference/realtime) for new event types — Mesh forwards them without code changes.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/realtime-audio
