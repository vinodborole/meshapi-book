---
type: Web Page
title: Models | Mesh API Docs
description: List and filter models with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/models
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Models

# Models

1 // List all models 2 all, _ := client.Models.List(ctx, meshapi.ListModelsParams{}) 3 4 // List free models only 5 free, _ := client.Models.Free(ctx) 6 7 // Filter by provider (Provider is *string) 8 provider := "amazon-bedrock" 9 bedrock, _ := client.Models.List(ctx, meshapi.ListModelsParams{ 10 Provider: &provider, 11 }) 12 13 for _, m := range bedrock { 14 fmt.Printf("%s: $%v/1k tokens\n", m.ID, m.Pricing.PromptUSDPer1K) 15 }

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/models
