---
type: Web Page
title: API reference - Mesh API
description: What the Mesh API covers, its base URL, authentication, and how to use
  the interactive explorer.
resource: https://developers.meshapi.ai/docs/reference/api-overview
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

## Key Features

- **Chat Completions** : OpenAI-compatible text, multimodal, and streaming inference.
- **Responses API** : Reasoning-style response generation for supported models.
- **Embeddings** : Dense vector creation for search and retrieval workflows.
- **Compare** : Run the same prompt across multiple models in one request.
- **Files and Batches** : Upload request bundles, create asynchronous jobs, and download results.
- **Model Discovery and Templates** : Browse available models and reuse prompt templates.

## Base URL

All API requests should be made to:
## Authentication

The Mesh API uses Bearer token authentication. You can obtain your API key from the Mesh developer dashboard.
## Versioning

The`/v1` above is a stable namespace, not a version — Mesh versions its contract **by date**, in a header:

`400 invalid_api_version` rather
than silently downgraded. See [API versioning](/docs/reference/api-versioning).

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/api-overview
