---
type: Web Page
title: Images & Vision - Mesh API
description: Send images to multimodal models and generate images through chat completions.
resource: https://developers.meshapi.ai/docs/capabilities/images-vision
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/chat/completions` for image input and image generation.
Use this guide for two common workflows:
- Send image input to a multimodal model
- Generate images from a text prompt

## Find image-capable models

Call`GET /v1/models` and inspect:
- `input_modalities` for image input support
- `output_modalities` for image output support

## Send image input

You can send images to multimodal models (like GPT-4o or Claude 3.5 Sonnet) by passing an array of content parts in the message. Mesh API supports two ways to provide images:
1. **Base64 encoded data** : Use`data:image/jpeg;base64,...` URLs.
2. **Public HTTP URLs** : Use standard`https://...` URLs.

`url` value with the data URL:
`"url": "data:image/jpeg;base64,iVBORw0KGgo..."`
## Generate images

Use the same chat completions endpoint with image-generation-capable models.
## Request tips

- Use `detail: "low"` for lower-cost image understanding when supported.
- Use hosted HTTPS URLs or `data:` URLs for image input.

## SDK coverage

- Node: `client.chat.completions.create(...)`
- Python: `client.chat.completions.create(...)`
- Go: `client.Chat.Completions.Create(...)`
- Java: `client.chat().completions().create(...)`

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/images-vision
