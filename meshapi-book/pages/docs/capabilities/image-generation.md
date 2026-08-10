---
type: Web Page
title: Image Generation - Mesh API
description: Generate and edit images using frontier vision models via a single API.
resource: https://developers.meshapi.ai/docs/capabilities/image-generation
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Generate an image

- curl
- Python

**Response:**

## Request fields

## Edit an image

Transform an existing image — edit it with a prompt (optionally guided by a mask) or remove its background — by uploading it to`POST /v1/images/edits`. Unlike generation, this endpoint uses **(file upload), not JSON.**

`multipart/form-data`
- curl
- Python

**Response:**

`gpt-image-1` always returns the result as a base64 data URI in the `url` field. Pass `response_format=b64_json` for a `b64_json` field instead (supported on DALL·E models).
### Operations

The`operation` field selects what to do. Support varies by provider:
Requesting an operation a provider doesn’t support returns 

`422`. A provider with no image-edit support at all returns `501`.
BytePlus supports 

`edit` only. `remove_background` is OpenAI-only.
### Request fields

`multipart/form-data`:
### More examples

Remove a background:
## Streaming

Some models support streaming image generation — partial image data is sent as the model renders it. Set`"stream": true`:
`text/event-stream`. Its main job is to keep the connection open during a long generation: the server emits an initial `processing` chunk, then SSE comment pings until the image is ready. The stream always ends with `data: [DONE]`.
### Keep-alive chunk sequence

## Available models

Check 

`GET /v1/models` for the full live list of enabled image models and their pricing.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/image-generation
