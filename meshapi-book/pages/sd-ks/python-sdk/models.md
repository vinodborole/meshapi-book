---
type: Web Page
title: Models | Mesh API Docs
description: List available models with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/models
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Models

# Models

Free models (`is_free=True`) cost $0 for both prompt and completion, useful for testing and light tasks. Paid models charge per token against your account balance.

Models billed in non-token units (speech, image, video) return `null` for the per-1M fields and publish their rate in `input_usd_per_unit` / `output_usd_per_unit`, labelled by `pricing_unit` (e.g. `per_second`, `per_image`). The SDK’s typed `ModelPricing` does not expose the per-unit fields yet — fetch the raw JSON (e.g. `GET /v1/public/models` with `requests`/`httpx`) if you need them.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/models
