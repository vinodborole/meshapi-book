---
type: Web Page
title: Embeddings - Mesh API
description: Create dense vector embeddings with the Mesh API and the official SDKs.
resource: https://developers.meshapi.ai/docs/capabilities/embeddings
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Quick start

- curl
- Node.js SDK
- Python SDK

## Request fields

## Batch input in a single request

You can embed multiple strings in one API call:`data` array matches the order of your input array.
## Response shape

## Choosing a model

Use`GET /v1/models` to inspect available embedding models and their pricing. For embeddings, the most useful fields are:
- `model_type`
- `context_length`
- `pricing`
- `is_free`

## SDK coverage

The official SDKs all expose first-class embeddings resources:
- Node: `client.embeddings.create(...)`
- Python: `client.embeddings.create(...)`
- Go: `client.Embeddings.Create(...)`
- Java: `client.embeddings().create(...)`

## Common errors

## Available models

Check 

`GET /v1/models` with the `type=embedding` filter to see the full live list of enabled embedding models.
## RAG with file uploads

For retrieval-augmented generation over your own documents, use the
[Files & RAG](/docs/capabilities/rag)workflow — upload files, trigger embedding, and search with

`POST /v1/files/search`. This is more scalable than calling the embeddings endpoint directly for large document corpora.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/embeddings
