---
type: Web Page
title: Embeddings | Mesh API Docs
description: Generate text embeddings with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/embeddings
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Embeddings

# Embeddings

1 from meshapi import EmbeddingsParams 2 3 result = client.embeddings.create( 4 EmbeddingsParams( 5 model="openai/text-embedding-3-small", 6 input=["hello world", "goodbye world"], 7 ) 8 ) 9 10 print(len(result.data[0].embedding))

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/embeddings
