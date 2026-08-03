---
type: Web Page
title: Edit an image | Mesh API Docs
description: 'OpenAI-compatible image editing endpoint. Accepts EITHER multipart/form-data
  (binary uploads: image, optional'
resource: https://developers.meshapi.ai/api-reference/mesh-api/images/edit-image
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Edit an image

### Authentication

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

### Response

The edited image(s).

OpenAI-compatible image editing endpoint.

Accepts EITHER `multipart/form-data` (binary uploads: `image`, optional
`mask`, `reference_images`) OR `application/json` whose image fields are
data-URL / base64 strings (or `{"url": ...}` objects) — the same convention
as the chat `image_url`. Remote http(s) URLs are rejected (mesh does not
server-side-fetch images); send a `data:image/<fmt>;base64,<data>` URL.

Supports `operation` in {edit, remove_background, upscale, outpaint, mix,
reframe, inpaint}. Each op is gated by the resolved model’s supports_image_<op>
flag (422 when unset / unsupported); the legacy provider matrix is a
transitional backstop for the original four ops.

`edit` (default): openai, byteplus, vertex — prompt + image (+ optional mask + refs).`remove_background`: openai, vertex — image only; prompt optional.`upscale`: vertex (Imagen) or byteplus (Seedream Smart Upscale) — image +
upscale_factor (x2|x4; Seedream maps x2→2k, x4→4k).`outpaint`: vertex — image + prompt + mask (defines expansion region).`mix`: Gemini image models + gpt-image (openai) — prompt + image + 1-3
reference_images (2-4 sources composed into one, sent as image[]); single
output.`reframe`: Gemini image models — image + aspect_ratio
(1:1|3:4|4:3|9:16|16:9|2:3|3:2); recomposes by outpainting to the target
ratio; single output.`inpaint`: openai, vertex — image + mask (canonical WHITE=edit; mesh
normalizes per provider). Optional mask_feather blends the result back
through the mask (Pillow; 501 if unavailable).
**Note:** The image response may not render in GitBook’s “Test it” panel.
To view the edited image, try this endpoint with Postman, curl, or another
HTTP client.

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/images/edit-image
