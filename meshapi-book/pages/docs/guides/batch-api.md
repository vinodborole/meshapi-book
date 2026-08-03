---
type: Web Page
title: Batch API | Mesh API Docs
description: Submit requests inline, create a batch, poll for completion, and read
  results inline.
resource: https://developers.meshapi.ai/docs/guides/batch-api
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Batch API

The Batch API is for asynchronous, high-volume inference jobs where you do not need an answer immediately.

## Workflow

1. Prepare a request bundle
2. Create a batch with `POST /v1/batches`
3. Poll `GET /v1/batches/{batch_id}` — results are included inline once complete

## 1 & 2. Create the batch

Pass your requests inline — no separate file upload required.

###### curl

###### Node.js SDK

###### Python SDK

###### Go SDK

###### Java SDK

### Request item fields

Each entry in `requests` supports:

The batch create call also accepts an optional `metadata` object (arbitrary key-value pairs) alongside `completion_window`.

### Limits

- A batch may not mix models — all requests must target the same model, or the create call returns `400 mixed_models` .
- You can have at most **10 batches in a non-terminal state** at once; an eleventh returns`429 batch_limit_exceeded` .

## 3. Poll and read results

Poll until `status` is a terminal value. When `completed`, the response includes a `results` array — no separate file download needed.

Common statuses:

- `validating`
- `in_progress`
- `finalizing`
- `completed`
- `failed`
- `cancelling`
- `cancelled`
- `expired`

## Notes

- All requests in a batch must use the same model.
- Batch jobs are best for throughput, not low-latency interactive use.
- Use `GET /v1/batches` to list recent batches and`POST /v1/batches/{batch_id}/cancel` to cancel one.
- Results are matched by `custom_id` — the output order is not guaranteed.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/batch-api
