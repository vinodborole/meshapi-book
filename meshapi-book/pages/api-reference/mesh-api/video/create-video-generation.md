---
type: Web Page
title: Create Video Generation | Mesh API Docs
description: 'Create a BytePlus Seedance video generation task. Returns immediately
  with {"id": "<taskid>"}. Poll GET /v1/video/generations/{id} until status is'
resource: https://developers.meshapi.ai/api-reference/mesh-api/video/create-video-generation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Video Generation

Create a BytePlus Seedance video generation task.
Returns immediately with ``{"id": "<task_id>"}``.
Poll ``GET /v1/video/generations/{id}`` until status is
``succeeded``, ``failed``, or ``expired``.
Pass ``callback_url`` to receive a POST notification when the task
status changes. We intercept the BytePlus callback, update our DB,
then forward the full task payload to your URL.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

model

content

callback_url

return_last_frame

service_tier

execution_expires_after

generate_audio

draft

resolution

ratio

duration

frames

seed

camera_fixed

watermark

safety_identifier

priority

### Response

Task created; poll the returned id for status

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

402

Payment Required Error

422

Unprocessable Entity Error

429

Too Many Requests Error

502

Bad Gateway Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/video/create-video-generation
