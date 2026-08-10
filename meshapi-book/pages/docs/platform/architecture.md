---
type: Web Page
title: Architecture - Mesh API
description: Behind the scenes of the Mesh AI Router.
resource: https://developers.meshapi.ai/docs/platform/architecture
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## High-Level Workflow

When you call`/v1/chat/completions`, the Mesh Router performs the following tasks:
1. **Authentication** : Validates your`rsk_` key, checks for active balance, and verifies spend caps.
2. **Provider Selection** : (If multiple providers offer the same model) Identifies the provider with the lowest current latency and cost.
3. **Secret Retrieval** : Fetches your encrypted provider keys from GCP Secret Manager (with memory-caching for sub-millisecond lookups).
4. **Request Transformation** : Converts your request into the provider-specific format (e.g., Anthropic Messages API or Google Vertex AI).
5. **Streaming/Buffering** : Pipes the response back to your client in real-time (supporting Server-Sent Events).
6. **Usage Logging** : Records the total tokens consumed, calculates the final cost in USD/INR, and updates your balance atomically.

## Resilience Features

- **Automatic Failover** : If an upstream provider (e.g., OpenAI) returns a 5xx error, Mesh can automatically retry the request with an alternative provider (e.g., Anthropic) if configured.
- **Circuit Breaking** : Mesh monitors error rates for each model-provider pair. If a provider starts failing, it’s temporarily removed from the routing pool.
- **Regional Optimization** : To minimize latency, Mesh routes requests to the nearest cloud region for global providers (Bedrock, Vertex AI).

## Security & Privacy

- **Key Scoping** : API keys are isolated per owner. Your keys can never be used by another account.
- **Zero-Storage Policy** : By default, Mesh does not store your prompt or completion content. Only metadata (tokens, model name, duration) is logged for billing and debugging purposes.
- **Encrypted Secrets** : All upstream keys are stored in a hardware-isolated GCP Secret Manager with restricted IAM access.

# Citations

1. Source page: https://developers.meshapi.ai/docs/platform/architecture
