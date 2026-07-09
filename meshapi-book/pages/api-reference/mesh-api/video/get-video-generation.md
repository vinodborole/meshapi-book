---
type: Web Page
title: Get Video Generation | Mesh API Docs
description: Retrieve the status and result of a BytePlus video generation task. Returns
  from DB when the task is in a terminal state. Otherwise polls
resource: https://developers.meshapi.ai/api-reference/mesh-api/video/get-video-generation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Get Video Generation

Retrieve the status and result of a BytePlus video generation task.

Returns from DB when the task is in a terminal state. Otherwise polls BytePlus live and syncs the DB row.

Poll until `status` is one of:
`succeeded` — `content.video_url` is populated,
`failed` — check `error` field,
`expired` — task timed out.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

task_id

### Response

Task status and result when succeeded

id

model

status

error

created_at

updated_at

content

seed

resolution

ratio

duration

frames

framespersecond

generate_audio

safety_identifier

priority

draft

draft_task_id

service_tier

execution_expires_after

usage

### Errors

401

Unauthorized Error

422

Unprocessable Entity Error

502

Bad Gateway Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/video/get-video-generation
