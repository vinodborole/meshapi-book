---
type: Web Page
title: Embeddings | Mesh API Docs
description: Generate vector embeddings with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/embeddings
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Embeddings

# Embeddings

1 model := "openai/text-embedding-3-small" 2 resp, err := client.Embeddings.Create(ctx, meshapi.EmbeddingsParams{ 3 Model: &model, 4 Input: []string{"The quick brown fox", "jumped over the lazy dog"}, 5 }) 6 7 for _, emb := range resp.Data { 8 fmt.Printf("Vector length: %d\n", len(emb.Embedding.Floats())) 9 }

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/embeddings
