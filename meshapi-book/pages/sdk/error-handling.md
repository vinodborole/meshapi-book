---
type: Web Page
title: Error Handling - Mesh API
description: Catch typed API errors, read error codes, and configure automatic retries.
resource: https://developers.meshapi.ai/sdk/error-handling
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Catching errors

- Python
- Node.js
- Go

`MeshAPIError` from inside the iterator.
## Error field names by language

## Error codes

## Automatic retries

Non-streaming`create()` requests are retried automatically on `429`, `502`, `503`, and `504` with exponential backoff. The `Retry-After` header is respected on 429 responses.
**Streams never retry automatically.**If a stream drops mid-response, the client raises an error and you need to restart the request manually. See

[Streaming — Stream recovery](/sdk/streaming).

`max_retries` is not the only knob. The full retry policy — status codes, backoff
bounds, `Retry-After` handling, network-error retry — plus a client-side **model fallback chain**and retry/fallback logging are covered in

[Resilience](/sdk/resilience).

# Citations

1. Source page: https://developers.meshapi.ai/sdk/error-handling
