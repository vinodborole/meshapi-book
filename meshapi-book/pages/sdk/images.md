---
type: Web Page
title: Image Generation - Mesh API
description: Generate images from text prompts.
resource: https://developers.meshapi.ai/sdk/images
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Generate an image

- Python
- Node.js
- Go

## Getting the image bytes

Where the image data lands depends on the model and`response_format`. With the
default (or `response_format="url"`), some models — including `openai/gpt-image-1`
— return the image **inline as a**rather than in

`data:` URI in `url``b64_json`.
Pass `response_format="b64_json"` to always get base64 in `b64_json`, or use the
helper below to get raw bytes regardless of shape:
- Python
- Go

`url` is a remote `http(s)` link (fetch it yourself).
## Response fields

`openai/gpt-image-1` is the recommended image generation model. Additional image generation models may be available depending on your account.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/images
