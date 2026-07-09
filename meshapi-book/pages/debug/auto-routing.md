---
type: Web Page
title: Auto Routing | Mesh API Docs
description: Why auto didn't route, which model actually ran, the extra classifier
  cost, and added latency.
resource: https://developers.meshapi.ai/debug/auto-routing
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Auto Routing

Auto Routing (`model: "auto"`) classifies each request and picks a model for
you. Most confusion is about *which* model ran, *what* you were billed, and
*where* it’s supported. See Auto Routing for the full guide.

## ‘auto’ didn’t route on my endpoint

Auto Routing only works on a few inference endpoints:

Setting `model: "auto"` on any other endpoint (images, video, audio) won’t
route — use a concrete model ID there.

## Which model actually served my request?

Mesh injects the resolved model into the response, but **where** depends on
streaming:

- **Non-streaming**— in the body:- `x_resolved_model_id`and- `x_auto_routed`.
- **Streaming**(- `stream: true`) — as- **HTTP response headers**- *before*the SSE stream:- `X-Auto-Routed`and- `X-Resolved-Model-Id`. They are- **not**in the body, which is why people miss them on streams.

## It keeps picking a cheap/default model (e.g. gpt-4o-mini)

The router never blocks a request on its own failure. If the internal
classifier times out or returns an unknown model, it **falls back to a reliable
default** (e.g. `openai/gpt-4o-mini`).

Check the response for:

If you always land on the fallback, the classifier is failing — not your prompt.

## Costs are higher than the resolved model alone

With Auto Routing you’re billed for **two** things: the tokens of the resolved
model **plus** the tokens consumed by the internal classifier model. Both show
up in the usage dashboard. For cost-sensitive or high-volume paths, pin a
specific model instead.

## I need control over the model choice

`auto` gives the gateway full discretion. If you need determinism:

- Pin a single model ID, or
- Supply a `models`fallback array to constrain the set of models a request may use, instead of`"auto"`.

## First-token latency went up

The classification step adds an extra internal LLM call before your request is
forwarded, which increases time-to-first-token. For latency-sensitive
interactive use, pin a model rather than using `auto`.

## Still stuck?

See the Mesh API error reference
or email **contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/auto-routing
