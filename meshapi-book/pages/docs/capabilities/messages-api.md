---
type: Web Page
title: Messages API (Anthropic-compatible) - Mesh API
description: Point an Anthropic SDK at Mesh and reach every model in the catalog through
  /v1/messages.
resource: https://developers.meshapi.ai/docs/capabilities/messages-api
timestamp: '2026-08-17T07:05:01.394536+00:00'
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

## Streaming

Set`stream: true` for Anthropic’s SSE event sequence — `message_start`,
`content_block_start`, `content_block_delta`, `content_block_stop`,
`message_delta`, `message_stop`. Anthropic’s SDKs consume it unchanged.
Token counts arrive on 

`message_delta`, not `message_start`. Anthropic reports
input tokens on the first frame; Mesh only knows them once the upstream response
completes, so `message_start.usage` carries zeros and `message_delta.usage` carries
the real figures. SDKs that accumulate usage across the stream end up with the
correct totals.
## Request fields

## Which endpoint should I use?

## Related

- [Claude Code](/docs/capabilities/claude-code) — point the CLI at Mesh
- [Tool Calling](/docs/capabilities/tool-calling) — the function-calling cycle
- [Responses API](/docs/capabilities/responses-api) — reasoning effort and hosted tools
- [Available Models](/docs/reference/models-list) — every model this endpoint can reach

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/messages-api
