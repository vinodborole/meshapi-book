---
type: Web Page
title: Error Handling | Mesh API Docs
description: Handle API errors in the Go SDK.
resource: https://developers.meshapi.ai/sd-ks/go-sdk/error-handling
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Error Handling

# Error Handling

The SDK returns `MeshAPIError` for structural API failures.

1 resp, err := client.Chat.Completions.Create(ctx, params) 2 if err != nil { 3 var svcErr *meshapi.MeshAPIError 4 if errors.As(err, &svcErr) { 5 fmt.Printf("Error %d (%s): %s\n", svcErr.Status, svcErr.Code, svcErr.Message) 6 fmt.Println("Request ID:", svcErr.RequestID) 7 8 if svcErr.Code == "rate_limit_exceeded" { 9 fmt.Printf("Retry after %v seconds\n", svcErr.RetryAfterSeconds) 10 } 11 } else { 12 // Network or context error 13 log.Fatal(err) 14 } 15 } 

## Common Error Codes

| Code | Status | Description | 
|---|---|---|
| `unauthorized` | 401 | Invalid or missing API key | 
| `rate_limit_exceeded` | 429 | You have hit a rate limit | 
| `spend_limit_exceeded` | 402 | Your spend cap has been reached | 
| `model_not_found` | 404 | The requested model is not available | 
| `upstream_error` | 500 | An error occurred with the provider |

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/go-sdk/error-handling
