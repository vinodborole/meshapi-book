---
type: Web Page
title: Image Generation | Mesh API Docs
description: Parameter differences across providers, response formats, streaming,
  and edits.
resource: https://developers.meshapi.ai/debug/image-generation
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

# Image Generation

Image generation runs over `POST /v1/images/generations` (and
`POST /v1/images/edits`). See [Image Generation](/image-generation) for the
full reference.

## size / quality value rejected

Valid values differ by model. `quality` is `low`/`medium`/`high` for GPT image,
`hd`/`standard` for DALL·E, or `auto`. `size` accepts dimensions or aspect
ratios (e.g. `1024x1024`, `1792x1024`) or `auto`. `n` must be **1–10**.

## I got base64 back when I asked for a URL (Vertex/Imagen)

Vertex AI (Imagen) **always returns  b64_json** regardless of

`response_format`, and does **not**return token usage. Handle both encodings, and don’t rely on

`usage` being present for Imagen.## output_format has no effect

`output_format` (`png` / `jpeg` / `webp`) and `output_compression` are
**OpenAI-only**. Other providers ignore them.

## Streaming request looks stuck

With `stream: true` you get a `text/event-stream`. Early chunks are keep-alive
(`status: "processing"`) plus `: ping` comments to hold the connection open
during long generations. The stream always ends with `data: [DONE]` — wait for
the chunk carrying `data[].url` (or `b64_json`).

## 501 Not Implemented on an edit

`POST /v1/images/edits` is `multipart/form-data`, and the `operation` (`edit`,
`remove_background`, `upscale`, `outpaint`, `inpaint`, `mix`, `reframe`) must be
supported by the chosen model — an unsupported operation returns ** 501**.

`inpaint` also requires a `mask`.## Still stuck?

See the [Mesh API error reference](/debug/mesh-api#error-code-reference)
or email ** contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/image-generation
