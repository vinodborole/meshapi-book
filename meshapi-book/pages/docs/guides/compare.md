---
type: Web Page
title: Compare | Mesh API Docs
description: Compare multiple models with a single prompt.
resource: https://developers.meshapi.ai/docs/guides/compare
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Compare

Use `POST /v1/chat/compare` when you want to run the same conversation across multiple models and inspect the results side by side.

## How it works

- **Fan-out**: All requested models are called concurrently. The total wall-clock time is roughly that of the slowest model, not the sum of all models.
- **Error isolation**: If a single model fails or times out (hard timeout of 120s), the others continue unaffected. Partial results are returned with a- `partial: true`flag.
- **Synthesis (default)**: After all models respond, a separate comparison LLM analyzes the responses and produces a structured evaluation covering accuracy, completeness, clarity, and a recommendation.
- **Skip synthesis (optional)**: By setting- `skip_comparison: true`, you can skip the synthesis step and receive only the raw model outputs. This is useful for parallel streaming UIs that perform their own comparison.
- **Rate limiting and Billing**: The entire comparison counts as a single request against your rate limits (RPM/RPD). However, billing tracks each model call plus the comparison call as separate usage events (N+1 events).
- **Streaming**: Two streaming modes are available by setting- `stream: true`. With synthesis enabled, fan-out is non-streaming, but the final comparison text is streamed token-by-token. If- `skip_comparison: true`is set, each fan-out model streams its tokens in real-time concurrently, tagged by model name.

## Basic request

###### curl

###### Node.js SDK

###### Python SDK

###### Go SDK

###### Java SDK

## Request fields

## Response shape

The response includes:

- the compared model list
- one result per model
- optional synthesized comparison text
- latency and request metadata

## Streaming (SSE)

Set `"stream": true` to receive a `text/event-stream` with typed events. There are two streaming modes:

### Mode 1: With comparison (`skip_comparison: false`, default)

Fan-out models are non-streaming (full response collected per model), then the comparison LLM streams token-by-token.

### Mode 2: Skip comparison (`skip_comparison: true`)

Each fan-out model streams tokens in real time concurrently, tagged by model name. No comparison LLM is called.

## SDK coverage

- Node: `client.compare.create(...)`
- Python: `client.compare.create(...)`
- Go: `client.Compare.Create(...)`
- Java: `client.compare().create(...)`

Streaming compare is also available in the SDKs through their compare stream methods when you want incremental events instead of a single final JSON response.

## When to use compare

- Evaluate multiple candidate models for a task
- Compare cost/quality trade-offs before choosing a default
- Build internal prompt evaluations with a stable request shape

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/compare
