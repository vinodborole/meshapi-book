---
type: Web Page
title: Compare (Multi-model) | Mesh API Docs
description: Compare model outputs in parallel with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/compare-multi-model
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

Compare (Multi-model)

Compare (Multi-model)

# Compare (Multi-model)

Compare multiple models side-by-side with a single call.

1 chunkCh, errCh := client.Compare.Stream(ctx, meshapi.CompareParams{ 2 Models: []string{ 3 "openai/gpt-4o-mini", 4 "anthropic/claude-3-haiku", 5 "google/gemini-1.5-flash", 6 }, 7 Messages: []meshapi.ChatMessage{{Role: "user", Content: "Write a poem about Go."}}, 8 }) 9 10 // CompareStreamEvent is a map[string]interface{}. A per-model token chunk 11 // carries "delta" (the token) and "model" (which model produced it). 12 for event := range chunkCh { 13 if delta, ok := event["delta"].(string); ok { 14 fmt.Printf("[%v]: %s\n", event["model"], delta) 15 } 16 } 17 if err := <-errCh; err != nil { 18 // handle stream error 19 }

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/compare-multi-model
