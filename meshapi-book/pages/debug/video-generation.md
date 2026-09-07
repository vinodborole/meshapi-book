---
type: Web Page
title: Video Generation - Mesh API
description: Fix video generation task failures, stalled polling, and expired result
  URLs.
resource: https://developers.meshapi.ai/debug/video-generation
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

**asynchronous**— the create call returns a task ID, not a video. Most issues come from treating it like a synchronous endpoint, or from input constraints on the model. See

[Video Generation](/docs/capabilities/video-generation)for the full guide.

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

Which inputs a model takes varies model by model, and the capability flags on`GET /v1/models` are the answer — `supports_video_reference_video` and
`supports_video_reference_audio` for these two. See
[Finding out what a model accepts](/docs/capabilities/video-generation#finding-out-what-a-model-accepts).

- A clip or an audio track is accepted by far fewer models than an image is. Check the flag before you rely on one.
- Some models will not take **audio as the only media** — send a reference image or a
clip with it.
- Use `duration`**or**`frames` , not both.

## Request too large / Base64 failures

**Total request body: 32 MiB (33,554,432 bytes)**— a hard platform limit on every Mesh endpoint. Requests above it are rejected at the edge

*before*they reach the API, so the response is a plain HTML

`413 Request Entity Too Large` page with no
Mesh error envelope and no request ID. See
[Rate Limits & Spend Caps](/docs/getting-started/rate-limits#request-body-size). Per-file caps are the model’s, and they are only reachable via public URLs, not Base64 — see

[What you can send](/docs/capabilities/video-generation#what-you-can-send)for the widest ceilings. Base64 inflates a payload by ~33%, so a

**~24 MB**file on its own already exceeds the 32 MiB body limit. Do

**not**Base64-encode large files. Use a public URL instead — it must be reachable

**without authentication**, since the model fetches it — and if you do inline small files, the data URI must include the MIME prefix (e.g.

`data:video/mp4;base64,...`).
## 422 on the create call

Two different causes, and the message tells them apart.
**The**List the ones that are with

`model` ID isn’t a video-generation model.`GET /v1/models?type=video`, or check `supports_video_generation` on the model you had
in mind.
**The model can’t take one of your**— the message names it and says it “would be dropped and the generation billed anyway”.

`content` items`supports_video_generation` is
`true` here; the flag you need is the one for that input, e.g.
`supports_video_last_frame`. Check the six on `GET /v1/models`, or
`GET /v1/models/{model_id}/providers` to see whether any route takes it. Full detail:
[Finding out what a model accepts](/docs/capabilities/video-generation#finding-out-what-a-model-accepts).

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

- `502` — an upstream error while generating.`503` — video generation temporarily unavailable. Both are usually transient; retry with backoff.

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/video-generation
