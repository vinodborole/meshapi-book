---
type: Web Page
title: Classify content against the content policy | Mesh API Docs
resource: https://developers.meshapi.ai/api-reference/mesh-api/moderations/create-moderation
timestamp: '2026-07-20T09:25:48.943332+00:00'
---

# Classify content against the content policy

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

input

Text, list of texts, or list of multimodal (text/image_url) items.

model

Moderation model to use. Defaults to omni-moderation-latest.

### Response

Moderation result(s).

### Errors

401

Unauthorized Error

403

Forbidden Error

422

Unprocessable Entity Error

429

Too Many Requests Error

500

Internal Server Error

503

Service Unavailable Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/moderations/create-moderation
