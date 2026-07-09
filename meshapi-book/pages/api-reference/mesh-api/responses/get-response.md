---
type: Web Page
title: Get Response | Mesh API Docs
description: Get the status of a background response job. Polls the upstream provider
  for the current status. Usage logging and
resource: https://developers.meshapi.ai/api-reference/mesh-api/responses/get-response
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Get Response

Get the status of a background response job.

Polls the upstream provider for the current status. Usage logging and billing fire automatically on the first terminal observation. When status is “completed” the full response object (including output) is returned pass-through from the upstream provider.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

response_id

### Response

Successful Response

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/responses/get-response
