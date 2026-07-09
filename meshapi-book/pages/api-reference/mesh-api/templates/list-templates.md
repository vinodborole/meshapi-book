---
type: Web Page
title: List Templates | Mesh API Docs
description: List templates for the caller's org. Admin/owner can filter by memberowner.
resource: https://developers.meshapi.ai/api-reference/mesh-api/templates/list-templates
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# List Templates

List templates for the caller’s org. Admin/owner can filter by member_owner.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

team_id

member_owner

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

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/templates/list-templates
