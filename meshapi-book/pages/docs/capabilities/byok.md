---
type: Web Page
title: Bring Your Own Keys (BYOK) - Mesh API
description: Use your own upstream provider credentials instead of the shared system
  keys.
resource: https://developers.meshapi.ai/docs/capabilities/byok
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

BYOK is opt-in and optional. Running on Mesh’s provider credentials is the right choice for most accounts.

## Supported providers

Every provider has a matching test endpoint — 

`POST /v1/provider-keys/test/{provider}` validates credentials without saving them, and `POST /v1/provider-keys/test/{pk_id}/{provider}` re-tests one you already registered. Validate before you rely on a key.
## Platform fee

BYOK traffic carries a platform fee of
**5% of upstream cost**. The first

**1,000,000 tokens**each month are fee-free.

## Adding a provider key

### Via Dashboard

Open the
[Dashboard](https://app.meshapi.ai)and select

**BYOK**from the sidebar. The page lists each supported provider — select the one you want to configure. Inside the provider page, use the

**Add**button to register a key. You can add multiple keys per provider, but only one can be active at a time — enable the one you want to use.

### Via API

Use the provider-specific registration endpoints. For example, to add an AWS Bedrock key using IAM credentials:
## Using a provider key

Once registered, requests for that provider automatically use your key based on the team associated with your API key. No changes to your API call are required — the routing is transparent.
## Fallback behavior

By default, if your key fails due to an auth error or rate limit, the request transparently retries using the shared system credentials. To disable this, set`allow_fallback: false` when registering the key:
`X-BYOK-Fallback-Triggered` header. Don’t confuse it with `X-Mesh-Routing-Fallback`, which is about **providers and models**rather than credentials — see

[Retry & Fallback](/docs/platform/retry-and-fallback). Both can appear on the same response.

## Key management

## Credential format by provider

## AWS Bedrock — IAM credentials

AWS Bedrock — IAM credentials

`bedrock:InvokeModel`, `bedrock:InvokeModelWithResponseStream`, and `bedrock:ListFoundationModels` permissions on the models you intend to use.
## AWS Bedrock — Bedrock API key

AWS Bedrock — Bedrock API key

`region` must match the AWS region where the key was created and cannot be changed after creation.
## Google Vertex AI

Google Vertex AI

Provide the service account key fields directly (as downloaded from the GCP console or The service account needs the 

`gcloud iam service-accounts keys create`):`roles/aiplatform.user` role and `aiplatform.endpoints.predict` permission on your GCP project. Omit `region` or set it to `"global"` to allow requests to run in any available region.
## OpenAI

OpenAI

## Troubleshooting

Common BYOK failures and their causes:
BYOK errors surface with the 

**provider’s**HTTP status, not Mesh’s. A`401` here means *your provider key*was rejected — not your`rsk_` key.
[Troubleshooting → BYOK](/debug/byok).

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/byok
