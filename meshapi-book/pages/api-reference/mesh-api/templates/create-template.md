---
type: Web Page
title: Create Template | Mesh API Docs
description: Create a template scoped to the authenticated user's owner.
resource: https://developers.meshapi.ai/api-reference/mesh-api/templates/create-template
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Template

Create a template scoped to the authenticated user's owner.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

name

description

system

messages

model

params

variables

team_id

### Response

Successful Response

id

name

owner

org_id

team_id

is_global

description

system

messages

model

params

variables

creator_name

creator_email

created_at

updated_at

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/templates/create-template
