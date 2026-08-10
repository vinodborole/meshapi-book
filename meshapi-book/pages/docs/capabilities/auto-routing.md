---
type: Web Page
title: Auto Routing - Mesh API
description: Dynamically route requests to the best model.
resource: https://developers.meshapi.ai/docs/capabilities/auto-routing
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`model: "auto"` on any inference request. The gateway will classify the request using an internal LLM and select the most appropriate model from the live registry, forwarding the request transparently.
This requires no client-side logic beyond setting the `model` field to `"auto"`.
## Supported endpoints

Auto Routing is supported across the following inference endpoints:
## Basic request

Just replace your specific model ID with`"auto"`:
- curl
- Node.js SDK
- Python SDK
- Go SDK
- Java SDK

## Response metadata

When a request is automatically routed, Mesh API injects metadata into the response so you know which model was actually used.
### Non-streaming requests

The metadata is included directly in the response body.
### Streaming requests

For streaming requests (`stream: true`), the metadata is included as HTTP response headers before the SSE stream begins:
## Fallback behavior

The Auto Router is designed to never block a request due to its own failure. If the internal classification model fails to respond in time or returns an unknown model, the gateway will automatically fall back to a reliable default model (e.g.,`openai/gpt-4o-mini`).
## Billing

When using the Auto Router, you are billed for the tokens consumed by the
*resolved*model that actually served the request,

**as well as the tokens consumed by the internal classifier model**. Both will appear in your usage dashboard.

## When to use auto routing

- **Rapid prototyping** — skip the model selection decision early in development
- **Mixed-complexity workloads** — let the gateway route simple queries to cheap models and hard ones to frontier models
- **A/B testing** — observe which models get selected for your actual traffic

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/auto-routing
