---
type: Web Page
title: RAG (Files & Search) - Mesh API
description: Fix file uploads, stuck embeddings, and searches that return nothing.
resource: https://developers.meshapi.ai/debug/rag
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**upload → embed → search**— and most issues come from acting before a step finishes. See

[RAG](/docs/capabilities/rag)for the full guide.

## Upload ‘works’ but the file is empty / not searchable

Uploading is
**two calls**:

`POST /v1/files` registers the file and returns a
`signed_url`; you then **PUT the actual bytes**to that URL.

- If you skip the PUT (or it failed), the file exists but has no content.
- The PUT goes to storage directly — **don’t** send your`Authorization` header on it.

## ‘signed_url’ rejected / expired

The`signed_url` expires **within a few minutes**. PUT your bytes immediately after registering the file. If it expired, register the file again to get a fresh URL.

## Search returns nothing right after upload

Embedding is asynchronous. A file is only searchable once`embedding_status` is `ready`.
- Poll `GET /v1/files/{file_id}` until`embedding_status` is`ready` (it moves`pending → queued → processing → ready` ).
- If it’s `failed` , re-trigger with`POST /v1/files/embed` (pass`"wait": true` to block for small files).
- If `embed: false` was set at upload, embedding never started — trigger it manually.

## Search returns irrelevant or too few results

- Raise `top_k` (1–50, default 5).
- An over-restrictive `filter` (metadata is**exact match** ) or`date_from` /`date_to` window can exclude everything — loosen them.
- A too-narrow `file_ids` list restricts search to those files only; omit it to search all your files.
- Metadata filters only match keys you set **at upload time** .

## 422 on upload

Check required fields on`POST /v1/files` — `file_name` and `mime_type` (e.g.
`application/pdf`, `text/plain`, `text/markdown`). The `mime_type` also drives
file-type detection.
## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/rag
