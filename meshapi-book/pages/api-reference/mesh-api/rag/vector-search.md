---
type: Web Page
title: Vector Search | Mesh API Docs
resource: https://developers.meshapi.ai/api-reference/mesh-api/rag/vector-search
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Vector Search

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

query

top_k

file_ids

Restrict to these file IDs

filter

Match on any metadata key/value pairs

date_from

Unix timestamp — only chunks created after this

date_to

Unix timestamp — only chunks created before this

### Response

Successful Response

results

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/rag/vector-search
