---
type: Web Page
title: Auto Routing | Mesh API Docs
description: Dynamically route requests to the best model.
resource: https://developers.meshapi.ai/docs/guides/auto-routing
timestamp: '2026-07-20T09:25:48.943332+00:00'
---

# Auto Routing

Mesh API’s Auto Router allows you to simply set `model: "auto"` on any inference request. The gateway will classify the request using an internal LLM and select the most appropriate model from the live registry, forwarding the request transparently.

This requires no client-side logic beyond setting the `model` field to `"auto"`.

## Supported endpoints

Auto Routing is supported across the following inference endpoints:

## Basic request

Just replace your specific model ID with `"auto"`:

###### curl

###### Node.js SDK

###### Python SDK

###### Go SDK

###### Java SDK

## Response metadata

When a request is automatically routed, Mesh API injects metadata into the response so you know which model was actually used.

### Non-streaming requests

The metadata is included directly in the response body.

If the internal classifier failed or timed out and a fallback model was used, additional fields are present:

### Streaming requests

For streaming requests (`stream: true`), the metadata is included as HTTP response headers before the SSE stream begins:

## Fallback behavior

The Auto Router is designed to never block a request due to its own failure. If the internal classification model fails to respond in time or returns an unknown model, the gateway will automatically fall back to a reliable default model (e.g., `openai/gpt-4o-mini`).

## Billing

When using the Auto Router, you are billed for the tokens consumed by the *resolved* model that actually served the request, **as well as the tokens consumed by the internal classifier model**. Both will appear in your usage dashboard.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/auto-routing
