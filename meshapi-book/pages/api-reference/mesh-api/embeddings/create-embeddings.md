---
type: Web Page
title: Create Embeddings | Mesh API Docs
resource: https://developers.meshapi.ai/api-reference/mesh-api/embeddings/create-embeddings
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Embeddings

### Authentication

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

Text(s) to embed. Accepts a string, list of strings, list of token IDs, list of token ID lists, or a list of MultimodalEmbeddingInput objects (for BytePlus multimodal embedding models).

Model ID to use for embedding, e.g. `perplexity/pplx-embed-v1-4b`.

Number of dimensions for the output embedding vector (model-dependent).

Format of the returned embedding. Defaults to `float`.

Intended use of the embedding, e.g. `query` or `document`. Some models use this to apply asymmetric embedding.

Provider routing preferences. Pass a provider slug string (e.g. `'perplexity'`) or a `ProviderPreferences` object to control fallback and ordering behaviour.

End-user identifier for abuse monitoring (forwarded to OpenRouter).

Sparse vector switch for BytePlus text embeddings. Pass {“type”: “enabled”} to return both dense and sparse vectors, or {“type”: “disabled”} for dense only.

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/embeddings/create-embeddings
