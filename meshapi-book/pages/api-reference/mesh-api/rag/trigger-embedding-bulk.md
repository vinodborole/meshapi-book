---
type: Web Page
title: Trigger Embedding Bulk | Mesh API Docs
description: Manually enqueue embedding jobs for one or more files. Each file must
  have uploadstatus=ready and embeddingstatus=pending or failed.
resource: https://developers.meshapi.ai/api-reference/mesh-api/rag/trigger-embedding-bulk
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Trigger Embedding Bulk

Manually enqueue embedding jobs for one or more files.

Each file must have upload_status=ready and embedding_status=pending or failed. Returns a per-file result — successes are ‘queued’, failures include an error message.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

file_ids

wait

metadata

### Response

Successful Response

results

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/rag/trigger-embedding-bulk
