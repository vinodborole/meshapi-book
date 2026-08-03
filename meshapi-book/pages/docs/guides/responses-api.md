---
type: Web Page
title: Responses API | Mesh API Docs
description: Reasoning effort, hosted server-side tools, and background jobs — the
  /v1/responses endpoint and how it differs from chat completions.
resource: https://developers.meshapi.ai/docs/guides/responses-api
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Responses API

`/v1/responses` is an OpenAI-compatible alternative to chat completions, aimed at reasoning models and **hosted tools** the provider runs on your behalf. Use it when you need controllable reasoning effort, provider-hosted capabilities like web search or code execution, or long-running work that outlives a single HTTP connection.

For ordinary chat, `/v1/chat/completions` remains the right endpoint.

## Reasoning effort

Reasoning models can be told how much thinking to spend:

Higher effort produces more reasoning tokens — better results on hard problems, at higher cost and latency. Reasoning tokens are billed.

Chat completions accepts a flat `reasoning_effort` field for the same purpose. The nested `reasoning: { effort }` form belongs to the Responses API.

## Hosted tools

Unlike [function calling](/docs/guides/tool-calling), where *you* execute the function, hosted tools run on the provider’s side and return their results directly.

**`web_search_preview` forces the request into background mode.** Web search is slow enough that a dropped HTTP connection would lose the billing record, so MeshAPI upgrades these requests to `background: true` automatically.

The practical consequence: you get back a **queued job**, not a completed answer, even though you didn’t ask for one. Code that expects a synchronous result will break. Poll `GET /v1/responses/{id}` for the outcome.

## Background jobs

Set `background: true` for work that shouldn’t be tied to a live connection:

You receive a response id immediately, then poll:

`GET /v1/responses` lists your jobs.

You can have at most **10 active background jobs** at once; jobs in a terminal state don’t count. Credit is reserved when a job is queued and settled when it finishes, so a queued job reduces your spendable balance before it produces anything.

## Provider-specific fields

Several fields are honoured only on particular upstreams — `thinking`, `caching`, `store`, `expire_at`, and `context_management` among them.

An unsupported field is **silently ignored**, not rejected. Sending `store: true` to a provider that doesn’t implement it returns a perfectly normal response in which nothing was stored. Verify behaviour on the specific model you are targeting rather than assuming a field took effect.

Other supported controls include `previous_response_id` for continuing a conversation, `max_tool_calls` (1–10) to bound hosted tool usage, `include` for requesting extra output, and `text.format` for structured output.

## Related

- [Tool Calling](/docs/guides/tool-calling) — functions your own code executes
- [Web Search](/docs/guides/web-search) — the standalone search endpoint, billed differently
- [Structured Output](/docs/guides/structured-output)

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/responses-api
