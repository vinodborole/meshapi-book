---
type: Web Page
title: RAG (Files & Search) | Mesh API Docs
description: Upload files, embed them, and run vector search with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/rag-files-search
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# RAG (Files & Search)

RAG (Files & Search)

# RAG (Retrieval-Augmented Generation)

Upload documents, generate embeddings, and query them semantically — all through `client.RAG`.

## Quick upload

The `UploadFile` convenience method handles both the init call and the PUT to the signed URL in one step:

## Two-step upload

Use `InitUpload` when you want to control the PUT yourself (e.g. streaming large files):

## Trigger embedding

## Poll until ready

## Search

### Search options

## List files

## RAG chat

Combine search results with a chat completion:

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/rag-files-search
