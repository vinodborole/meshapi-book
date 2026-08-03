---
type: Web Page
title: Models | Mesh API Docs
description: List and filter models with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/models
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Models

# Models

Models billed in non-token units (speech, image, video) return `null` for the per-1M fields and publish their rate in `input_usd_per_unit` / `output_usd_per_unit`, labelled by `pricing_unit` (e.g. `per_second`, `per_image`). The SDK’s `ModelPricing` struct does not declare the per-unit fields yet, so they are dropped during decoding — call the endpoint with `net/http` and decode the raw JSON if you need them.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/models
