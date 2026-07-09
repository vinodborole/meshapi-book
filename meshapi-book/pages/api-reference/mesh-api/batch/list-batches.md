---
type: Web Page
title: List Batches | Mesh API Docs
description: List batch jobs for the authenticated key's owner. Returns MeshAPI's
  own BatchJob records (unified view across all providers)
resource: https://developers.meshapi.ai/api-reference/mesh-api/batch/list-batches
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# List Batches

List batch jobs for the authenticated key’s owner.

Returns MeshAPI’s own BatchJob records (unified view across all providers)
in the OpenAI list format: `{"object": "list", "data": [...], "has_more": bool}`.
Supports cursor-based pagination via `after` (batch_id) and `limit`.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

after

limit

### Response

Successful Response

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/batch/list-batches
