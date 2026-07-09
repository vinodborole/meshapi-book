---
type: Web Page
title: Error Handling | Mesh API Docs
description: Handle errors and retries in the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/error-handling
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Error Handling

# Error Handling

Mid-stream errors (sent as SSE frames before `[DONE]`) raise the same `MeshAPIError` from inside the iterator.

## Retry and backoff

The client automatically retries `GET` and non-streaming `POST` / `PATCH` requests on status codes `429`, `502`, `503`, `504` with exponential backoff (default: 3 retries, base delay 500 ms, max 30 s, with jitter). The `Retry-After` header is respected on 429 responses.

## Streaming failure recovery

**Streams do not retry.** If a connection drops mid-stream, a `MeshAPIError` with `error_code="stream_interrupted"` is raised. Catch it and restart a new request:

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/error-handling
