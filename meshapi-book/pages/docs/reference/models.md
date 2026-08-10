---
type: Web Page
title: Models - Mesh API
description: Understanding model IDs, providers, capabilities, and how to choose the
  right model.
resource: https://developers.meshapi.ai/docs/reference/models
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`provider/model-name` slug — the same format used in all inference requests.
Looking for the full catalog? 

[Available Models](/docs/reference/models-list)lists every model Mesh can route to today, with live pricing and context lengths.
## What a model is

Mesh acts as a router, forwarding your standardized API calls to an expansive list of underlying foundational models. A
**model**is a distinct neural network trained by an AI organization (OpenAI, Google, Anthropic, Meta, and others). Each model handles a different maximum

**context length**— the number of tokens it can process in a single request. Fast, small models may have smaller limits but respond almost instantly; large models can handle whole documents and are optimized for reasoning, at higher latency and cost. When choosing one, weigh three things:

- **Cost** — do you need high intelligence, or just rapid categorization?
- **Latency** — lighter models offer much lower time-to-first-token.
- **Context length** — passing an entire codebase or large PDF needs a long-context model.

## Model ID format

## Listing available models

`pricing` object carries a field per billing dimension the model supports —
cache read/write, long context, batch, audio, per-image, and a flat `request_usd`
where one applies. Dimensions the model does not bill for are `null`. See
[Pricing](/docs/getting-started/pricing)for how the units work.

### Query parameters

### Additional endpoints

## Choosing a model

### By task type

### By cost

Free models (`is_free: true`) cost $0 for both prompt and completion tokens. Check the Dashboard → **Models**tab to filter for free models by provider. Paid model pricing varies widely — a frontier reasoning model may cost 100× more per token than a lightweight model. Check

`pricing.prompt_usd_per_1m` and `pricing.completion_usd_per_1m` in the API response for live pricing.
### Let the gateway decide

Set`"model": "auto"` and the gateway classifies your prompt and picks an appropriate model automatically. See [Auto Routing](/docs/capabilities/auto-routing).

## Model capabilities

`supports_system_prompt` defaults to `true`. Check the relevant flag before sending a request that depends on a specific capability — an unsupported feature is generally rejected by the upstream rather than silently downgraded.
## Provider routing

Each model is backed by one upstream provider. The provider determines authentication, latency, and regional availability:
The model registry is live — models can be enabled or disabled by the platform team. Always use 

`GET /v1/models` at runtime rather than hardcoding a static model list.

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/models
