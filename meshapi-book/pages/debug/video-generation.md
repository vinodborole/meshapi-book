---
type: Web Page
title: Video Generation - Mesh API
description: Fix video generation task failures, stalled polling, and expired result
  URLs.
resource: https://developers.meshapi.ai/debug/video-generation
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**asynchronous**— the create call returns a task ID, not a video. Most issues come from treating it like a synchronous endpoint, or from input constraints on the model. See

[Video Generation](/docs/capabilities/video-generation)for the full reference.

## The POST response has no video, just an id

That’s expected.`POST /v1/video/generations` returns `{"id": "t-..."}`
immediately. The video isn’t ready yet.
- Poll `GET /v1/video/generations/{id}` until`status` is terminal,**or**
- Pass a `callback_url` to receive the result by webhook.

## HTTP 200 but the task failed

A task can fail
*after*it was accepted. In that case the GET returns

**HTTP 200**with

`status: "failed"` and an `error` object — the failure is in the
body, not the HTTP status.
`status` (`succeeded` / `failed` / `expired` / `cancelled`),
not just the HTTP code. Common `error.code` values: `content_policy_violation`,
`invalid_input`.
## List endpoint shows a stale status

`GET /v1/video/generations` (the list) is served from Mesh’s database and does
**not**refresh from the provider. For a live status on an in-progress task, call

`GET /v1/video/generations/{id}` — that one forces an upstream sync.
## Video / audio input rejected

Input modality support is model-specific (BytePlus Seedance):
- **Video** and**audio** inputs are**Seedance 2.0 series only** . Older Seedance models accept text and image only.
- **Audio cannot be the sole input** — you must also include a reference image or video in the`content` array.
- Use `duration`**or**`frames` , not both.

## Request too large / Base64 failures

**Total request body: 32 MiB (33,554,432 bytes)**— a hard platform limit on every Mesh endpoint. Requests above it are rejected at the edge

*before*they reach the API, so the response is a plain HTML

`413 Request Entity Too Large` page with no
Mesh error envelope and no request ID. See
[Rate Limits & Spend Caps](/docs/getting-started/rate-limits#request-body-size). Per-file caps (BytePlus Seedance; other providers may differ):

- Video input: max **50 MB** . Audio: max**15 MB** ,**2–15 s** per clip, up to**3 clips** , ≤**15 s** combined.

*provider*accepts — they are only reachable via public URLs, not Base64. Base64 inflates payloads by ~33%, so a

**~24 MB**file on its own already exceeds the 32 MiB body limit. Do

**not**Base64-encode large files. Use a public URL instead — reachable by the provider

**without authentication**— and if you do inline small files, the data URI must include the MIME prefix (e.g.

`data:video/mp4;base64,...`).
## 422 — model not supported for video

A`422` on the create call means the `model` ID isn’t a video-generation model.
Use a supported model such as `byteplus/dreamina-seedance-2-0`.
## Task shows ‘expired’

The task didn’t finish before its expiry window. The default is`execution_expires_after` = **172800 s (48 h)**. Raise it on the create request if you expect long jobs, or resubmit.

## My webhook never fired

Callbacks are
**fire-and-forget**with a

**10-second timeout**and are

**not retried**.

- Return a `2xx` quickly (do heavy work after responding). A slow or failing endpoint means you miss the event.
- Only terminal states (`succeeded` ,`failed` ,`expired` ) trigger a callback.
- As a safety net, also poll `GET /v1/video/generations/{id}` — Mesh deduplicates usage logging, so you’re never billed twice.

## 5xx from the video service

- `502` — upstream provider error.`503` — video service temporarily unavailable. Both are usually transient; retry with backoff.

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/video-generation
