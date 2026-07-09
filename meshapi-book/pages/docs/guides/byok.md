---
type: Web Page
title: BYOK | Mesh API Docs
description: Learn how to use your existing AI provider keys with Mesh API.
resource: https://developers.meshapi.ai/docs/guides/byok
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# BYOK

# Bring Your Own Keys (BYOK)

Mesh API supports both using Mesh API credits (platform credentials) and the option to bring your own provider keys (BYOK).

Using provider keys enables direct control over rate limits and costs via your provider account (e.g., AWS Bedrock, Google Vertex AI).

## How it Works

When you configure a BYOK key, Mesh API securely stores the reference and uses it for requests routed through that provider on your behalf.

By default, requests will use your BYOK key. If your key fails (e.g., due to authentication errors or rate limits), the request will fall back to using Mesh API’s platform credentials, unless you have disabled fallback for that key.

### Key Fallback

You can control the fallback behavior for each key:

- **Fallback Enabled (Default)**: If your key fails or hits a rate limit, Mesh API will attempt to fulfill the request using shared platform credentials.
- **Exclusive Use**: You can disable fallback to ensure that requests for that provider ONLY use your key. If your key fails, the request will fail.

## Platform Fee

A platform fee of **4%** applies to BYOK requests. The first 1,000,000 tokens are included at no platform fee.

## Supported Providers

BYOK is supported for the following providers. Here is what you need to configure each:

### OpenAI

For OpenAI, you only need to provide your **API key**.

### AWS Bedrock

You can configure Bedrock using one of two methods:

**Method 1: Bedrock API Keys**
Simply provide your Bedrock API key as a string. Note that these keys are tied to a specific AWS region.

**Method 2: AWS Credentials (JSON)**
Alternatively, you can provide a JSON object with your AWS IAM credentials and region. The `credential_type` distinguishes IAM credentials (`iam_credentials`) from a Bedrock API key (`bedrock_api_key`):

Ensure your IAM user or role has the following permissions policy attached:

### Google Vertex AI

To use Vertex AI, you must provide your Google Cloud **service account key** in JSON format. The key should contain all standard fields, and you can optionally include a `"region"` field to specify the deployment region (defaults to `"global"` if omitted).

Example service account key with optional region:

Ensure the service account has the **Vertex AI User** (`roles/aiplatform.user`) role or at least the `aiplatform.endpoints.predict` permission.

## Debugging BYOK Issues

If your BYOK requests fail, you can debug the issue by checking the response or viewing logs. Common issues include:

- **401 Unauthorized**: Your provider API key is invalid or revoked.
- **403 Forbidden**: Your key lacks permissions for the requested model (e.g., AWS IAM policy missing- `bedrock:InvokeModel`,- `bedrock:InvokeModelWithResponseStream`, or- `bedrock:ListFoundationModels`).
- **429 Too Many Requests**: You have hit the rate limit on your provider account.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/byok
