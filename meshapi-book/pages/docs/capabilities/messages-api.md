---
type: Web Page
title: Messages API (Anthropic-compatible) - Mesh API
description: Point an Anthropic SDK at Mesh and reach every model in the catalog through
  /v1/messages.
resource: https://developers.meshapi.ai/docs/capabilities/messages-api
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/messages` implements Anthropic’s Messages API shape. If your code already
speaks that format — through the Anthropic SDK or a hand-rolled client — you can
switch the base URL to Mesh and keep the rest of the integration unchanged.
The endpoint is 

**not restricted to Anthropic models**. Any model this gateway serves is reachable through it, so an existing Anthropic client can call an OpenAI, Google, or open-weight model without changing its request shape.
[remains the recommended entry point — it has the widest feature coverage.](/docs/getting-started/quickstart)

`/v1/chat/completions`
## Authentication

Both header styles are accepted:`x-api-key` form is what Anthropic’s own SDKs send, which is why an unmodified
client works once the base URL is pointed at Mesh.
## Basic request

- curl
- Python (Anthropic SDK)
- Node.js (Anthropic SDK)

## Request fields

## Which endpoint should I use?

## Related

- [Tool Calling](/docs/capabilities/tool-calling) — the function-calling cycle
- [Responses API](/docs/capabilities/responses-api) — reasoning effort and hosted tools
- [Available Models](/docs/reference/models-list) — every model this endpoint can reach

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/messages-api
