---
type: Web Page
title: Streaming - Mesh API
description: Stream chat completions token by token, cancel mid-stream, and handle
  stream errors.
resource: https://developers.meshapi.ai/sdk/streaming
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Basic streaming

The streaming API differs by language: Python uses a separate`stream()` method, Node.js passes `stream: true` to `create()`, and Go returns channels.
- Python
- Node.js
- Go

`stream()` is a separate method from `create()`. It returns a sync iterator.
## Async streaming (Python)

Use`AsyncMeshAPI` to stream in async contexts.
## Cancelling a stream

- Python
- Node.js
- Go

Break out of the iterator at any point — no cleanup needed.

## Stream recovery (Python)

Streams do not automatically retry mid-stream. If the connection drops, a`MeshAPIError` with `error_code="stream_interrupted"` is raised. Catch it and restart:
Non-streaming 

`create()` requests retry automatically on `429`, `502`, `503`, and `504`. Streams never retry automatically — the client would have to re-generate content already received.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/streaming
