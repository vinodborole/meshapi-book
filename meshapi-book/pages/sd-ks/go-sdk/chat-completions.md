---
type: Web Page
title: Chat Completions | Mesh API Docs
description: Chat with models using the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/chat-completions
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Chat Completions

# Chat Completions

1 model := "openai/gpt-4o-mini" 2 resp, err := client.Chat.Completions.Create(ctx, meshapi.ChatCompletionParams{ 3 Model: &model, 4 Messages: []meshapi.ChatMessage{ 5 {Role: "system", Content: "You are a helpful assistant."}, 6 {Role: "user", Content: "Explain quantum entanglement."}, 7 }, 8 }) 9 10 if err == nil { 11 fmt.Println(*resp.Choices[0].Message.Content) 12 } 

## Streaming

Streaming uses Go channels to receive chunks as they arrive:

1 chunkCh, errCh := client.Chat.Completions.Stream(ctx, params) 2 for chunk := range chunkCh { 3 if len(chunk.Choices) > 0 && chunk.Choices[0].Delta != nil { 4 fmt.Print(*chunk.Choices[0].Delta.Content) 5 } 6 } 7 if err := <-errCh; err != nil { 8 // Handle stream error 9 }

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/chat-completions
