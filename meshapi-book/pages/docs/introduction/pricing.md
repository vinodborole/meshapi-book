---
type: Web Page
title: Pricing Details | Mesh API Docs
description: Understand how pricing and billing works for MeshAPI models.
resource: https://developers.meshapi.ai/docs/introduction/pricing
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Pricing Details

# Detailed Pricing

MeshAPI provides a simplified token-based pricing structure to ensure you only pay for exactly what you consume.

## How Token Pricing Works

Like many foundational model APIs, usage is calculated per token. We display pricing based on blocks of **1 million tokens** (the industry-standard “per-1M” convention).

There are two primary components to pricing:

- **Prompt Pricing (`prompt_usd_per_1m`):** The cost per 1 million tokens of context sent*to* the model.
- **Completion Pricing (`completion_usd_per_1m`):** The cost per 1 million tokens generated*by* the model.

Since different models require different computational resources, pricing varies heavily. Some lighter models are exceptionally cheap per token, while heavy reasoning models cost more.

## Non-Token Pricing Units

Not every model bills by tokens. Speech, image, and video models publish their rate in the unit they actually bill in — check the `pricing` object’s `pricing_unit` field (for example `per_second`, `per_1k_chars`, `per_image`, `per_video`, `per_hour`).

For these models `prompt_usd_per_1m` / `completion_usd_per_1m` are `null` (a per-token figure would be meaningless), and the real rate is published in:

- **`input_usd_per_unit`:** USD per one`pricing_unit` of input (e.g.`0.002` with`pricing_unit: per_hour` = $0.002/hour of audio).
- **`output_usd_per_unit`:** USD per one`pricing_unit` of output (e.g.`0.14` with`pricing_unit: per_video` = $0.14/video).

For token-priced models (`pricing_unit: per_1m_tokens`) the per-unit fields simply mirror the per-1M values.

## Free Tier vs. Paid Models

We sort models into two distinct buckets to make cost administration easy:

1. **Free Models:** These models are completely free to use (`is_free = true` ) and cost $0 for both prompt and completion. They are exceptional for testing your application routing, standardizing prompts, and light tasks.
2. **Paid Models:** These charge a variable rate. Paid models typically include advanced reasoning capabilities and larger context sizes.

## Pre-paid Balances

Before using Paid models, you must load funds onto your account balance. Your balance is debited micro-fractions of a cent per token processed. Once your balance reaches zero (or a key’s spend cap is hit), paid API requests return an `HTTP 402` response with error code `spend_limit_exceeded` until you top up or raise the cap.

# Citations

1. Source page: https://developers.meshapi.ai/docs/introduction/pricing
