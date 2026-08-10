---
type: Web Page
title: Video Generation - Mesh API
description: Generate videos from text, images, video clips, and audio using async
  video generation models.
resource: https://developers.meshapi.ai/docs/capabilities/video-generation
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST` endpoint creates a task and returns a task ID immediately. You then poll `GET /v1/video/generations/{id}` until the task reaches a terminal state, or receive the result passively via a webhook callback.
**Base URL:**

`https://api.meshapi.ai`
**Auth:**

`Authorization: Bearer rsk_<your-key>` on all requests.
## Endpoints

## Create a Video Generation Task

`POST /v1/video/generations`
Submits a video generation request. Returns immediately with `{"id": "<task_id>"}`.
### Request body

### Content items

The`content` array lets you combine different input modalities. Each item has a `type` field and a corresponding payload field.
#### Text prompt

#### Image input (reference / first frame)

Pass a publicly accessible URL or a Base64-encoded image.
#### Video input (reference video)

**Seedance 2.0 series only.**Pass a publicly accessible URL or a Base64-encoded video.

#### Audio input

**Seedance 2.0 series only.**Pass a publicly accessible URL or a Base64-encoded audio file.

### Input size limits

These limits apply to BytePlus Seedance models. Other providers may differ.
**Video input**

- Max file size: **50 MB** per video

**Audio input**

- Supported formats: `wav` ,`mp3`
- Duration per clip: **2–15 seconds** ; up to**3 reference audio clips** allowed, with a total combined duration of no more than**15 seconds**
- Max file size: **15 MB** per audio file

**Max total request body size: 32 MiB (33,554,432 bytes).**This is a hard platform limit on every Mesh endpoint, enforced at the edge — an oversized request is rejected before it reaches the API and returns a plain HTML

`413 Request Entity Too Large`
page rather than a Mesh error envelope. See [Rate Limits & Spend Caps](/docs/getting-started/rate-limits#request-body-size). For public URLs, the file must be reachable by the provider’s servers without authentication. For Base64 input, the data URI must include the MIME type prefix (e.g.

`data:video/mp4;base64,...`).
### Seedance input support matrix

### Response

`id` to poll for status.
### Example — text to video

- curl
- Python
- Node.js

### Example — image to video

### Example — video with audio input

## Get Task Status

`GET /v1/video/generations/{id}`
Returns the current state of a task. For non-terminal tasks, Mesh API fetches the latest status from the upstream provider and syncs its database before responding.
### Task statuses

Once a task reaches a terminal status (

`succeeded`, `failed`, `expired`, `cancelled`), subsequent GET calls are served from the Mesh API database without hitting the upstream provider.
### Response fields

### Example — polling until complete

- Python
- curl

### Succeeded response

### Failed response

## List Tasks

`GET /v1/video/generations`
Returns tasks stored in Mesh API’s database for your API key. This endpoint does **not**refresh task status from the upstream provider — use

`GET /v1/video/generations/{id}` to force a live sync for an in-progress task.
### Query parameters

Results are ordered by 

`created_at` descending (newest first).
### Response

## Webhooks

Instead of polling, you can pass a`callback_url` in the create request. Mesh API will `POST` the full task payload to your URL when the task reaches a terminal status (`succeeded`, `failed`, or `expired`).
### How it works

1. You pass `"callback_url": "https://yourapp.example.com/webhooks/video"` in the create request.
2. Mesh API intercepts the completion notification from the upstream provider, updates its database, and then forwards the completed task payload to your `callback_url` .
3. Your endpoint receives a `POST` with a JSON body containing the final task state.

### Webhook payload

The payload is identical to the response from`GET /v1/video/generations/{id}`:
### Handling the webhook

Your endpoint should:
1. Return a `2xx` response quickly. Mesh API fires the callback as fire-and-forget with a 10-second timeout — a slow response won’t block task completion on our side, but it will not be retried.
2. Identify the task from `id` in the payload.
3. Check `status` to decide what to do — download`content.video_url` on success, log or surface`error` on failure.

### Webhook vs polling — when to use each

You can use both simultaneously: pass a 

`callback_url` for production delivery and still poll `GET /v1/video/generations/{id}` as a fallback. Mesh API deduplicates usage logging so you are never billed twice.
## Pricing

Video generation pricing is token-based and varies by the type of output:
The exact per-token rates for each model are listed on the 

[Pricing](/docs/getting-started/pricing)page.

## Error reference

When a task itself fails (HTTP 200, 

`status: "failed"`), the `error.code` and `error.message` fields describe the reason from the provider (e.g. `content_policy_violation`, `invalid_input`).

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/video-generation
