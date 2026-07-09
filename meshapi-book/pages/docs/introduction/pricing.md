---
type: Web Page
title: Pricing Details | Mesh API Docs
description: Understand how pricing and billing works for MeshAPI models.
resource: https://developers.meshapi.ai/docs/introduction/pricing
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Pricing Details

# Detailed Pricing

MeshAPI provides a simplified token-based pricing structure to ensure you only pay for exactly what you consume.

## How Token Pricing Works

Like many foundational model APIs, usage is calculated per token. We display pricing based on blocks of **1,000 tokens**.

There are two primary components to pricing:

- **Prompt Pricing (**The cost per 1,000 tokens of context sent- `prompt_usd_per_1k`):- *to*the model.
- **Completion Pricing (**The cost per 1,000 tokens generated- `completion_usd_per_1k`):- *by*the model.

Since different models require different computational resources, pricing varies heavily. Some lighter models are exceptionally cheap per token, while heavy reasoning models cost more.

## Free Tier vs. Paid Models

We sort models into two distinct buckets to make cost administration easy:

- **Free Models:**These models are completely free to use (- `is_free = true`) and cost $0 for both prompt and completion. They are exceptional for testing your application routing, standardizing prompts, and light tasks.
- **Paid Models:**These charge a variable rate. Paid models typically include advanced reasoning capabilities and larger context sizes.

## Pre-paid Balances

Before using Paid models, you must load funds onto your account balance. Your balance is debited micro-fractions of a cent per token processed. Once your balance reaches zero (or a key’s spend cap is hit), paid API requests return an `HTTP 402` response with error code `spend_limit_exceeded` until you top up or raise the cap.

# Citations

1. Source page: https://developers.meshapi.ai/docs/introduction/pricing
