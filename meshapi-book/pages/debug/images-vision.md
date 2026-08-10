---
type: Web Page
title: Images & Vision - Mesh API
description: Fix image input failures — URL fetches, base64 payloads, and unsupported
  models.
resource: https://developers.meshapi.ai/debug/images-vision
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**input**goes through

`POST /v1/chat/completions` as content parts. See
[Images & Vision](/docs/capabilities/images-vision)for the full guide.

## The model ignores or rejects my image URL

**Not all models accept images via public URL.**Some providers require the image to be

**base64-encoded**. If a URL isn’t working, switch to base64 for maximum compatibility.

## Base64 image not recognized

The data URL must include the MIME-type prefix, e.g.`data:image/jpeg;base64,iVBORw0KGgo...`. A bare base64 string without the
`data:image/...;base64,` prefix won’t be detected as an image.
## Model doesn’t support image input at all

Only multimodal models accept images. Confirm support with`GET /v1/models` and
check `input_modalities` (for image input) and `output_modalities` (for image
output) before sending.
## Image understanding costs more than expected

Image tokens add up. Where the model supports it, set`detail: "low"` on the
`image_url` part for cheaper, lower-resolution understanding.
## Wrong content shape

Image input uses an
**array of content parts**in the message — a

`text` part
plus an `image_url` part — not a plain string `content`. Sending a string with
a URL in it won’t attach the image.
## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/images-vision
