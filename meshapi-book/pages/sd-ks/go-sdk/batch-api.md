---
type: Web Page
title: Batches | Mesh API Docs
description: Process large workloads with the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/batch-api
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

Go SDKBatchesAsk a question|Copy page|View as Markdown|More actionsBatches Process thousands of requests asynchronously. Results are returned inline in the poll response — no separate file download required. 1. Create a Batch 1// CompletionWindow is *string.2completionWindow := "24h"3batch, err := client.Batches.Create(ctx, meshapi.CreateBatchParams{4 Requests: []meshapi.BatchRequestItem{5 {6 CustomID: "req-1",7 Body: map[string]interface{}{8 "model": "openai/gpt-5.4",9 "messages": []map[string]string{10 {"role": "user", "content": "Say hi."},11 },12 },13 },14 },15 CompletionWindow: &completionWindow,16}) 2. Monitor and Retrieve 1// Poll until completed2status, _ := client.Batches.Get(ctx, batch.ID)34// Results are included inline once status is "completed"5if status.Status == "completed" {6 for _, result := range status.Results {7 fmt.Println(result.CustomID, result.Response.Body.Choices[0].Message.Content)8 }9}

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/batch-api
