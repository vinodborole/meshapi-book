---
type: Web Page
title: MCP Server | Mesh API Docs
description: Connect any MCP-compatible agent or client to Mesh and call models, tools,
  RAG, and more — authenticated with your Mesh API key.
resource: https://developers.meshapi.ai/agent/mcp/mcp-server
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# MCP Server

Mesh runs a built-in **[Model Context Protocol (MCP)](https://modelcontextprotocol.io/)** server, so any MCP-compatible agent or client — Claude Code, Claude Desktop, Cursor, or your own agent framework — can use Mesh’s models and tools directly, without writing any Mesh-specific HTTP code.

The server exposes the Mesh API surface as MCP **tools**: an agent can run chat completions, reason with the Responses API, generate and edit images, create embeddings, moderate content, search the web, query your RAG files, manage prompt templates, and check your balance — all authenticated with your existing Mesh API key.

The MCP server is a thin layer over the same routes as the REST API. Every tool call runs the identical authentication, rate-limit, spend-cap, balance, and usage-logging pipeline as a direct API call, and is billed to the same key.

## Connection details

You connect with the **same `rsk_` API key** you use for the REST API — no separate credential or OAuth flow. Pass it in the `Authorization` header when registering the server with your client.

## Connect a client

###### Claude Code

###### Claude Desktop / Cursor

###### Python (mcp SDK)

Then, in a session, ask Claude to use the Mesh tools (e.g. “use mesh-api to draft a reply with `openai/gpt-4o-mini`”).

Keep your `rsk_` key secret — it authorizes billable calls against your account. Treat an MCP client config that embeds the key like any other secret store, and rotate the key from the dashboard if it leaks.

## Available tools

Tools mirror the REST API one-to-one. Inputs are OpenAI-shaped, so anything you know from the [API reference](/api-reference) carries over directly.

### Inference

### RAG & search

### Catalog & account

### Prompt templates

The server also exposes a **resource**, `mesh://models`, that returns the full model catalog as JSON — useful for clients that consume MCP resources rather than calling `list_models`.

## Example: give an agent a document Q&A tool

A typical agent flow — upload a file, wait for embeddings, then answer questions grounded in it — is three tool calls:

## Authentication & billing

- **Auth:** the`Authorization: Bearer rsk_...` header is validated before any tool runs. A missing or invalid key returns**401** ; a suspended key returns**403** .
- **Limits & billing:** billed tool calls inherit the key’s per-key rate limits, spend cap, model allow-list, and credit balance — exactly as a direct REST call. If a call would exceed a limit or your balance is insufficient, the tool returns an error carrying the Mesh message (e.g.*“Insufficient balance.”* ).
- **Free tools:** the catalog/account reads (`list_models` ,`list_voices` ,`get_balance` ), the RAG file reads (`list_files` ,`get_file` ,`upload_file` ), template management, and`router_select` are not billed. Everything under**Inference** , plus`web_search` and`file_search` , is billed.
- **Usage tracking:** every billed tool call is logged to your usage history just like a REST request, so MCP traffic shows up in the dashboard and usage reports.

## Limitations

Tool results are returned as a single payload — there is no token-by-token streaming of a `chat`/`responses` result over MCP. For streaming, use the REST API or an SDK.

The following Mesh capabilities are **not yet exposed as MCP tools** — use the [REST API](/api-reference) or an SDK for them:

- Audio: text-to-speech and speech-to-text (transcription/translation)
- Video generation and the async Batch API
- Realtime audio (bidirectional WebSocket)
- Usage-report queries

# Citations

1. Source page: https://developers.meshapi.ai/agent/mcp/mcp-server
