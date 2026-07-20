---
type: Web Page
title: Update Template | Mesh API Docs
description: Update a template. Owner can edit own; org admin/owner can edit any org
  template.
resource: https://developers.meshapi.ai/api-reference/mesh-api/templates/update-template
timestamp: '2026-07-20T09:25:48.943332+00:00'
---

# Update Template

Update a template. Owner can edit own; org admin/owner can edit any org template.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

template_id

### Request

This endpoint expects an object.

name

description

system

messages

model

params

variables

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

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/templates/update-template
