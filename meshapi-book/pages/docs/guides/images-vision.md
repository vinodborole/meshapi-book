---
type: Web Page
title: Images & Vision | Mesh API Docs
description: Send images to multimodal models and generate images through chat completions.
resource: https://developers.meshapi.ai/docs/guides/images-vision
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

Images & Vision

Images & Vision

Mesh API uses `POST /v1/chat/completions` for image input and image generation.

Use this guide for two common workflows:

- Send image input to a multimodal model
- Generate images from a text prompt

## Find image-capable models

Call `GET /v1/models` and inspect:

- `input_modalities`for image input support
- `output_modalities`for image output support

## Send image input

You can send images to multimodal models (like GPT-4o or Claude 3.5 Sonnet) by passing an array of content parts in the message.

Mesh API supports two ways to provide images:

- **Base64 encoded data**: Use- `data:image/jpeg;base64,...`URLs.
- **Public HTTP URLs**: Use standard- `https://...`URLs.

[!WARNING] Not all models support passing images via public URL. Some models (especially from certain providers) require images to be base64 encoded. Check the provider documentation or use base64 encoding to ensure maximum compatibility.

Here is an example request sending an image via URL:

To send a base64 encoded image, replace the `url` value with the data URL:
`"url": "data:image/jpeg;base64,iVBORw0KGgo..."`

## Generate images

Use the same chat completions endpoint with image-generation-capable models.

The exact assistant payload may vary by model. Build against the documented response fields for the model you choose and the media format you request.

## Request tips

- Use `detail: "low"`for lower-cost image understanding when supported.
- Use hosted HTTPS URLs or `data:`URLs for image input.

## SDK coverage

- Node: `client.chat.completions.create(...)`
- Python: `client.chat.completions.create(...)`
- Go: `client.Chat.Completions.Create(...)`
- Java: `client.chat().completions().create(...)`

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/images-vision
