---
type: Web Page
title: Video Generation | Mesh API Docs
description: Async polling, task-level failures, input modality limits, size caps,
  and webhook delivery.
resource: https://developers.meshapi.ai/debug/video-generation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Video Generation

Async polling, task-level failures, input modality limits, size caps, and webhook delivery.

Video generation is **asynchronous** — the create call returns a task ID, not a
video. Most issues come from treating it like a synchronous endpoint, or from
input constraints on the model. See Video Generation for
the full reference.

## The POST response has no video, just an id

That’s expected. `POST /v1/video/generations` returns `{"id": "t-..."}`
immediately. The video isn’t ready yet.

- Poll `GET /v1/video/generations/{id}`until`status`is terminal,**or**
- Pass a `callback_url`to receive the result by webhook.

## HTTP 200 but the task failed

A task can fail *after* it was accepted. In that case the GET returns **HTTP
200** with `status: "failed"` and an `error` object — the failure is in the
body, not the HTTP status.

Always branch on `status` (`succeeded` / `failed` / `expired` / `cancelled`),
not just the HTTP code. Common `error.code` values: `content_policy_violation`,
`invalid_input`.

## List endpoint shows a stale status

`GET /v1/video/generations` (the list) is served from Mesh’s database and does
**not** refresh from the provider. For a live status on an in-progress task,
call `GET /v1/video/generations/{id}` — that one forces an upstream sync.

## Video / audio input rejected

Input modality support is model-specific (BytePlus Seedance):

- **Video**and- **audio**inputs are- **Seedance 2.0 series only**. Older Seedance models accept text and image only.
- **Audio cannot be the sole input**— you must also include a reference image or video in the- `content`array.
- Use `duration`**or**`frames`, not both.

## Request too large / Base64 failures

Size caps (BytePlus Seedance; other providers may differ):

- Video input: max **50 MB**. Audio: max**15 MB**,**2–15 s**per clip, up to**3 clips**, ≤**15 s**combined.
- Total request body: **64 MB**.

Do **not** Base64-encode large files — you’ll blow the 64 MB body limit. Use a
public URL instead. That URL must be reachable by the provider **without
authentication**, and Base64 data URIs must include the MIME prefix
(e.g. `data:video/mp4;base64,...`).

## 422 — model not supported for video

A `422` on the create call means the `model` ID isn’t a video-generation model.
Use a supported model such as `byteplus/dreamina-seedance-2-0`.

## Task shows ‘expired’

The task didn’t finish before its expiry window. The default is
`execution_expires_after` = **172800 s (48 h)**. Raise it on the create request
if you expect long jobs, or resubmit.

## My webhook never fired

Callbacks are **fire-and-forget** with a **10-second timeout** and are **not
retried**.

- Return a `2xx`quickly (do heavy work after responding). A slow or failing endpoint means you miss the event.
- Only terminal states (`succeeded`,`failed`,`expired`) trigger a callback.
- As a safety net, also poll `GET /v1/video/generations/{id}`— Mesh deduplicates usage logging, so you’re never billed twice.

## 5xx from the video service

- `502`— upstream provider error.- `503`— video service temporarily unavailable. Both are usually transient; retry with backoff.

## Still stuck?

See the Mesh API error reference
or email **contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/video-generation
