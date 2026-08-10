---
type: Web Page
title: Responses API - Mesh API
description: Reasoning effort, hosted server-side tools, and background jobs — the
  /v1/responses endpoint and how it differs from chat completions.
resource: https://developers.meshapi.ai/docs/capabilities/responses-api
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`/v1/responses` is an OpenAI-compatible alternative to chat completions, aimed at reasoning models and **hosted tools**the provider runs on your behalf. Use it when you need controllable reasoning effort, provider-hosted capabilities like web search or code execution, or long-running work that outlives a single HTTP connection. For ordinary chat,

`/v1/chat/completions` remains the right endpoint.
## Reasoning effort

Reasoning models can be told how much thinking to spend:
Chat completions accepts a flat 

`reasoning_effort` field for the same purpose. The nested `reasoning: { effort }` form belongs to the Responses API.
## Hosted tools

Unlike
[function calling](/docs/capabilities/tool-calling), where

*you*execute the function, hosted tools run on the provider’s side and return their results directly.

## Background jobs

Set`background: true` for work that shouldn’t be tied to a live connection:
`GET /v1/responses` lists your jobs.
You can have at most 

**10 active background jobs**at once; jobs in a terminal state don’t count. Credit is reserved when a job is queued and settled when it finishes, so a queued job reduces your spendable balance before it produces anything.
## Provider-specific fields

Several fields are honoured only on particular upstreams —`thinking`, `caching`, `store`, `expire_at`, and `context_management` among them.
Other supported controls include `previous_response_id` for continuing a conversation, `max_tool_calls` (1–10) to bound hosted tool usage, `include` for requesting extra output, and `text.format` for structured output.
## Related

- [Tool Calling](/docs/capabilities/tool-calling) — functions your own code executes
- [Web Search](/docs/capabilities/web-search) — the standalone search endpoint, billed differently
- [Structured Output](/docs/capabilities/structured-output)

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/responses-api
