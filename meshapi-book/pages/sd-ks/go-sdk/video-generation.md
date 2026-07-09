---
type: Web Page
title: Video Generation | Mesh API Docs
description: Submit and poll async video generation tasks with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/video-generation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Video Generation

# Video Generation

## Generate a Video

`client.Videos.Generate` sends `POST /v1/video/generations` and returns a `*CreateVideoGenerationResponse` containing the task ID.

## Poll Until Complete

## List Tasks

`client.Videos.List` sends `GET /v1/video/generations`.

## Retrieve a Task

`client.Videos.Retrieve` sends `GET /v1/video/generations/{task_id}`.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/video-generation
