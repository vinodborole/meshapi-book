---
type: Web Page
title: Auto Routing - Mesh API
description: Dynamically route requests to the best model.
resource: https://developers.meshapi.ai/docs/capabilities/auto-routing
timestamp: '2026-08-31T13:14:57.224524+00:00'
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

The Auto Router is designed to never block a request due to its own failure. Classification runs in tiers, and each tier falls through to the next rather than erroring:
1. **Primary classifier** — a fast, non-reasoning model selects the best match from the live registry.
2. **Secondary classifier** — if the primary fails, times out, or returns an id we do not serve, a second classifier on a*different provider* is tried, so one provider’s outage cannot take out both tiers.
3. **Default model** — if both classifiers fail, the request is served by a configured default model.

`x_auto_routed_fallback: true` and an `x_auto_routed_fallback_reason` (for example `classifier_timeout`), so a fallback is always visible rather than silent.
The classifier model and the default model are 

**different models with different prices**. A request that falls all the way through is served by the default — not by the classifier — so check`x_resolved_model_id` rather than assuming.
## Billing

When using the Auto Router, you are billed for the tokens consumed by the
*resolved*model that actually served the request,

**as well as the tokens consumed by the internal classifier model**. Both appear in your usage dashboard. The classifier’s cost is broken out on the response itself, so you can see it per request rather than only in aggregate:

`x_classifier_usage` is a list because a request that falls through to the secondary classifier bills for both attempts — each appears as its own entry.
## When to use auto routing

- **Rapid prototyping** — skip the model selection decision early in development
- **Mixed-complexity workloads** — let the gateway route simple queries to cheap models and hard ones to frontier models
- **A/B testing** — observe which models get selected for your actual traffic

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/auto-routing
