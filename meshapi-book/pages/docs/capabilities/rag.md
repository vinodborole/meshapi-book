---
type: Web Page
title: Files & RAG - Mesh API
description: Upload your documents, search them semantically, and ground AI answers
  in your own content — all through the Mesh API.
resource: https://developers.meshapi.ai/docs/capabilities/rag
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

1. **Upload** a file and get a`file_id`
2. **Wait** for embeddings to finish processing
3. **Search** with a natural-language query

## Step 1 — Upload a file

Call`POST /v1/files` to register the file. You get back a `file_id` and a short-lived `signed_url` — use the signed URL to PUT the actual bytes directly to storage.
- curl
- Python
- Node.js

### Init upload fields

### Init upload response

## Step 2 — Wait for embeddings

After the PUT completes, the API automatically chunks and embeds your file (because`embed: true`). You can poll `GET /v1/files/{file_id}` to watch progress.
- curl
- Python
- Node.js

### Status fields

Once 

`embedding_status` is `ready`, the file is ready to search.
## Step 3 — Search your files

Send a natural-language query to`POST /v1/files/search`. The API converts your query into a vector, finds the closest chunks, and returns the raw text with relevance scores.
- curl
- Python
- Node.js

### Search fields

### Search response

## End-to-end: RAG chat

Combine search results with a chat completion to answer questions from your documents.
- Python
- Node.js

## Filtering by metadata

Tag files at upload time and filter at search time — useful when you have documents from different departments, clients, or time periods.
## Re-triggering embeddings

If`embed` was set to `false` at upload time, or if embedding failed, you can kick it off manually:
`"wait": true` to block until all embeddings finish (useful for small files in scripts).
## List your files

`files` (array of file status objects), `total`, `limit`, and `offset`.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/rag
