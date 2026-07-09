---
type: Web Page
title: Get Template | Mesh API Docs
description: Get a single template by UUID. Must belong to the caller's owner or their
  org.
resource: https://developers.meshapi.ai/api-reference/mesh-api/templates/get-template
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Get Template

Get a single template by UUID. Must belong to the caller's owner or their org.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

template_id

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

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/templates/get-template
