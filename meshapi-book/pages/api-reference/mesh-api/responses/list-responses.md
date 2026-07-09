---
type: Web Page
title: List Responses | Mesh API Docs
description: List background response jobs for the authenticated key's owner. Returns
  MeshAPI's own records in OpenAI list format.
resource: https://developers.meshapi.ai/api-reference/mesh-api/responses/list-responses
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# List Responses

List background response jobs for the authenticated key’s owner.

Returns MeshAPI’s own records in OpenAI list format.
Supports cursor-based pagination via `after` (response_id) and `limit`.

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

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/responses/list-responses
