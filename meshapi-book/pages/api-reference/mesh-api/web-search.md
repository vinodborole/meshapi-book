---
type: Web Page
title: Web Search | Mesh API Docs
resource: https://developers.meshapi.ai/api-reference/mesh-api/web-search
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Web Search

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

query

The search query.

model

Native engine model id. Defaults to the server's configured model.

provider

Pin a specific engine. Omit to use native-first with Tavily fallback.

max_results

Maximum results to return.

search_depth

Tavily search depth; ignored by the native engine.

include_domains

Restrict results to these domains.

exclude_domains

Drop results from these domains.

include_answer

Ask the engine for a synthesized answer alongside results.

### Response

Successful Response

query

provider

answer

results

request_id

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/web-search
