---
type: Web Page
title: Image Generation | Mesh API Docs
description: Generate images with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/image-generation
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Image Generation

# Image Generation

Where the image lands depends on the model and `response_format`. With the default
(or `response_format="url"`), some models — including `openai/gpt-image-1` — return
the image **inline as a `data:` URI in `url`** rather than in `b64_json`. Use
`result.data[0].image_bytes()` to get raw bytes regardless of shape (it decodes
`b64_json` or a `data:` URL), or pass `response_format="b64_json"` for predictable
base64. A remote `http(s)` `url` raises — fetch it yourself.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/image-generation
