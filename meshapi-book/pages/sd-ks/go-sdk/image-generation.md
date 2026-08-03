---
type: Web Page
title: Image Generation | Mesh API Docs
description: Generate images with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/image-generation
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Image Generation

# Image Generation

Where the image lands depends on the model and `ResponseFormat`. With the default
(or `"url"`), some models — including `openai/gpt-image-1` — return the image
**inline as a `data:` URI in `URL`** rather than in `B64JSON`. Use
`resp.Data[0].Bytes()` to get raw bytes regardless of shape (it decodes `B64JSON`
or a `data:` URL), or set `ResponseFormat` to `"b64_json"` for predictable base64.
A remote `http(s)` `URL` returns an error — fetch it yourself.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/image-generation
