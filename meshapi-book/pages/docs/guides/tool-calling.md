---
type: Web Page
title: Tool Calling | Mesh API Docs
description: Let models call your functions — the request/response cycle, multi-turn
  flow, and the provider-specific details MeshAPI handles for you.
resource: https://developers.meshapi.ai/docs/guides/tool-calling
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Tool Calling

Tool calling lets a model ask your application to run a function and then continue with the result. MeshAPI implements the OpenAI-compatible `tools` / `tool_choice` interface on `/v1/chat/completions`, so existing OpenAI tool-calling code works unchanged.

## The cycle

1. You send a request with a `tools` array describing the functions available.
2. The model replies with `finish_reason: "tool_calls"` and one or more`tool_calls` .
3. You execute the function yourself and append the result as a `role: "tool"` message.
4. You send the conversation back; the model produces its final answer.

### 1. Declare the tools

### 2. The model asks for a call

`arguments` is a **JSON-encoded string**, not an object — parse it before use.

### 3. Return the result

Append the assistant message *and* a `tool` message carrying the matching `tool_call_id`:

Keep sending `tools` on the follow-up. Every `tool_call_id` must be answered by exactly one `tool` message.

## Controlling when tools are used

## Tool calling disables the response cache

Any request carrying a `tools` array is **never served from, or written to, the [gateway response cache](/docs/guides/caching)** — a tool call depends on live state, so replaying a stored one would be wrong.

This is silent. If you are relying on caching for cost and you add tools to a request, that saving disappears with no error and no header. It is the correct behaviour, but worth knowing before it shows up as a bill.

## Gemini thinking models

Gemini’s thinking models attach a **thought signature** to each tool call, and Vertex rejects a follow-up whose tool history is missing it. The OpenAI wire format has no slot for this, so MeshAPI exposes it in two places:

- on the tool call itself, as `tool_calls[].thought_signature`
- in a side channel, `message.reasoning_details` , for clients that strip unknown fields from`tool_calls`

**Echo it back verbatim** in the assistant message when you continue the conversation.

Some frameworks — LangChain among them — strip both fields automatically. MeshAPI detects the resulting rejection and retries once with the unsigned tool exchanges demoted to plain text, so your request still succeeds. You lose the thinking context on that turn, but you do not get an error. If you can preserve the signature, do; if your framework won’t, the fallback covers you.

## Related

- [Caching](/docs/guides/caching) — why tools bypass the response cache
- [Structured Output](/docs/guides/structured-output) — for shaping a response rather than calling code
- The **SDKs** tab has per-language tool-calling examples for Python, Node.js, and Go

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/tool-calling
