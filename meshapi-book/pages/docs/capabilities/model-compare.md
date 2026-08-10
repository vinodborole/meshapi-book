---
type: Web Page
title: Model Compare - Mesh API
description: Compare multiple models with a single prompt.
resource: https://developers.meshapi.ai/docs/capabilities/model-compare
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/chat/compare` when you want to run the same conversation across multiple models and inspect the results side by side.
## How it works

1. **Fan-out** : All requested models are called concurrently. The total wall-clock time is roughly that of the slowest model, not the sum of all models.
2. **Error isolation** : If a single model fails or times out (hard timeout of 120s), the others continue unaffected. Partial results are returned with a`partial: true` flag.
3. **Synthesis (default)** : After all models respond, a separate comparison LLM analyzes the responses and produces a structured evaluation covering accuracy, completeness, clarity, and a recommendation.
4. **Skip synthesis (optional)** : By setting`skip_comparison: true` , you can skip the synthesis step and receive only the raw model outputs. This is useful for parallel streaming UIs that perform their own comparison.
5. **Rate limiting and Billing** : The entire comparison counts as a single request against your rate limits (RPM/RPD). However, billing tracks each model call plus the comparison call as separate usage events (N+1 events).
6. **Streaming** : Two streaming modes are available by setting`stream: true` . With synthesis enabled, fan-out is non-streaming, but the final comparison text is streamed token-by-token. If`skip_comparison: true` is set, each fan-out model streams its tokens in real-time concurrently, tagged by model name.

## Basic request

- curl
- Node.js SDK
- Python SDK
- Go SDK
- Java SDK

## Request fields

## Response shape

The response includes:
- the compared model list
- one result per model
- optional synthesized comparison text
- latency and request metadata

## Streaming (SSE)

Set`"stream": true` to receive a `text/event-stream` with typed events. There are two streaming modes:
### Mode 1: With comparison (`skip_comparison: false`, default)

Fan-out models are non-streaming (full response collected per model), then the comparison LLM streams token-by-token.
### Mode 2: Skip comparison (`skip_comparison: true`)

Each fan-out model streams tokens in real time concurrently, tagged by model name. No comparison LLM is called.
## SDK coverage

- Node: `client.compare.create(...)`
- Python: `client.compare.create(...)`
- Go: `client.Compare.Create(...)`
- Java: `client.compare().create(...)`

## When to use compare

- **Model selection** — evaluate candidate models and find the best cost/quality trade-off for your task
- **Regression testing** — catch output quality regressions when a provider updates a model
- **Prompt engineering** — see how different phrasings affect output across several models at once
- **Provider comparison** — evaluate the same underlying model served by different providers
- **Internal evaluations** — build prompt evals on a stable request shape

All models in a compare request run in parallel — total latency is the slowest model’s response time, not the sum of all of them.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/model-compare
