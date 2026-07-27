---
type: Web Page
title: Query Usage Events | Mesh API Docs
description: Paginated per-request event history for the authenticated caller.
resource: https://developers.meshapi.ai/api-reference/mesh-api/usage/query-usage-events
timestamp: '2026-07-27T10:02:52.764636+00:00'
---

# Query Usage Events

Paginated per-request event history for the authenticated caller.

Authenticate with an rsk_ API key. Results are scoped to that single key — any org_id / member filters in the body are ignored. API-key reads are computed fresh.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

refresh

### Request

This endpoint expects an object.

org_id

since

until

model

status

key_id

team_id

member_user_id

end_user_id

limit

offset

### Response

Successful Response

events

total

limit

offset

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/usage/query-usage-events
