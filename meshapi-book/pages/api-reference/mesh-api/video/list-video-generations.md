---
type: Web Page
title: List Video Generations | Mesh API Docs
description: 'List BytePlus video generation tasks persisted in MeshAPI''s DB. This
  endpoint is intentionally DB-only: it does not poll BytePlus or update'
resource: https://developers.meshapi.ai/api-reference/mesh-api/video/list-video-generations
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# List Video Generations

List BytePlus video generation tasks persisted in MeshAPI’s DB.

This endpoint is intentionally DB-only: it does not poll BytePlus or update
task state. Use `GET /v1/video/generations/{id}` to refresh a non-terminal
task from the upstream provider.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

status

Filter by one or more task statuses, e.g. ?status=running&status=failed

model

Filter by one or more MeshAPI model ids

created_after

Only include tasks created at or after this ISO 8601 timestamp

created_before

Only include tasks created before this ISO 8601 timestamp

limit

offset

### Response

Persisted video generation tasks for the authenticated owner

data

has_more

total

limit

offset

object

### Errors

401

Unauthorized Error

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/video/list-video-generations
