---
type: Web Page
title: Embeddings | Mesh API Docs
description: Create dense vector embeddings with the Mesh API and the official SDKs.
resource: https://developers.meshapi.ai/docs/guides/embeddings
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Embeddings

The Embeddings API turns text into vectors you can use for semantic search, retrieval, clustering, and ranking.

## Quick start

###### curl

###### Node.js SDK

###### Python SDK

## Request fields

## Batch input in a single request

You can embed multiple strings in one API call:

The returned `data` array matches the order of your input array.

## Response shape

## Choosing a model

Use `GET /v1/models` to inspect available embedding models and their pricing. For embeddings, the most useful fields are:

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

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/embeddings
