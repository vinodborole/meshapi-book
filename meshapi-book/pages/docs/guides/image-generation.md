---
type: Web Page
title: Image Generation | Mesh API Docs
description: Generate images using OpenAI and Vertex AI models.
resource: https://developers.meshapi.ai/docs/guides/image-generation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Image Generation

# Image Generation

Mesh API exposes a standalone OpenAI-compatible image generation endpoint at `POST /v1/images/generations`, supporting models from OpenAI (`gpt-image-*`) and Vertex AI (Imagen).

Images can also be generated through `POST /v1/chat/completions` by setting the image modality on a chat request. The standalone `/v1/images/generations` endpoint below is the OpenAI-compatible path.

## Endpoint

`POST /v1/images/generations`

## Parameters

## Streaming

Setting `stream: true` returns a `text/event-stream` response that prevents the connection from closing during long-running generations.

The stream always ends with `data: [DONE]`.

### Keep-alive chunk sequence

## Response

`background`, `output_format`, `quality`, `size`, and `usage` are only present when the upstream provider returns them. Vertex AI (Imagen) does not return token usage. Vertex AI always returns `b64_json` regardless of `response_format`.

## Examples

### Standard generation

### Streaming

## Image editing

`POST /v1/images/edits`

Edit or transform an existing image. This is a `multipart/form-data` request — send the source image file plus the fields below. The `operation` determines the transform:

Common form fields: `image` (the source file), `prompt`, `model`, `operation`, `mask`, and `reference_images`. If the selected model does not support the requested operation, the endpoint returns `501 Not Implemented`.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/image-generation
