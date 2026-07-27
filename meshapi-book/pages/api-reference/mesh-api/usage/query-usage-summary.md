---
type: Web Page
title: Query Usage Summary | Mesh API Docs
description: Aggregate usage stats for the authenticated caller.
resource: https://developers.meshapi.ai/api-reference/mesh-api/usage/query-usage-summary
timestamp: '2026-07-27T10:02:52.764636+00:00'
---

# Query Usage Summary

Aggregate usage stats for the authenticated caller.

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

total_requests

successful_requests

error_requests

prompt_tokens

completion_tokens

total_tokens

total_cost_usd

by_model

total_platform_fee_usd

total_byok_platform_fee_usd

total_byok_cost_usd

total_byok_requests

total_platform_requests

total_byok_tokens

total_platform_tokens

by_model_total

cached_at

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/usage/query-usage-summary
