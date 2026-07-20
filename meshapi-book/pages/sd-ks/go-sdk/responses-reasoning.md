---
type: Web Page
title: Responses API (Reasoning) | Mesh API Docs
description: Query reasoning models with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/responses-reasoning
timestamp: '2026-07-20T09:25:48.943332+00:00'
---

Responses API (Reasoning)

Responses API (Reasoning)

# Responses API (Reasoning)

Access models like OpenAI’s o-series that require a specific reasoning interface.

Streaming is also supported via `client.Responses.Stream(ctx, params)`.

Use `client.Responses.List` and `client.Responses.Get` for persisted/background response jobs. A response returned from a synchronous `Create` call is not guaranteed to be retrievable later by ID.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/responses-reasoning
