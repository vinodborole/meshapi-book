---
type: Web Page
title: Router Select | Mesh API Docs
description: Resolve the Auto Router's model choice for a prompt without running inference.
resource: https://developers.meshapi.ai/api-reference/mesh-api/auto-router/router-select
timestamp: '2026-07-27T10:02:52.764636+00:00'
---

# Router Select

Resolve the Auto Router's model choice for a prompt without running inference.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

messages

api_type

exclude_models

candidate_models

### Response

Successful Response

model

auto_router

reasoning_effort

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/auto-router/router-select
