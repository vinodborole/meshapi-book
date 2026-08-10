---
type: Web Page
title: BYOK - Mesh API
description: 'Diagnose bring-your-own-key failures: credential shape, provider permissions,
  and silent fallback.'
resource: https://developers.meshapi.ai/debug/byok
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**configuration**problems: the credential is shaped wrong for the provider, the underlying account lacks a permission, or fallback is masking a broken key. See

[BYOK](/docs/capabilities/byok)for the full setup reference.

BYOK errors surface with the 

**provider’s**HTTP status, not Mesh’s. A`401`
here means *your provider key*was rejected — not your`rsk_` key.
## 401 — provider key invalid or revoked

The provider rejected the credential Mesh forwarded.
- The key was rotated, revoked, or copied with a typo / trailing whitespace.
- For **Bedrock API keys** , the key is tied to a**specific AWS region** — a key issued for one region won’t work for a model served from another.
- Re-enter the key in the dashboard and confirm it’s active in the provider console.

## 403 — key is valid but lacks permission for the model

Authentication worked, but the account isn’t authorized for that model or action.
- **AWS Bedrock** — the IAM user/role is missing the required actions. Attach`bedrock:InvokeModel` ,`bedrock:InvokeModelWithResponseStream` , and`bedrock:ListFoundationModels` .
- **Google Vertex AI** — the service account needs the**Vertex AI User** role (`roles/aiplatform.user` ) or at least`aiplatform.endpoints.predict` .
- The model may not be enabled/available in your provider account or region.

## Wrong credential shape (Bedrock / Vertex)

Each provider expects a specific format:
- **OpenAI** — just the API key string.
- **AWS Bedrock** —*either* a Bedrock API key string,*or* an IAM JSON object. If you use the JSON form,`credential_type` must match the contents:`iam_credentials` (with`access_key_id` ,`secret_access_key` ,`region` ) vs`bedrock_api_key` .
- **Google Vertex AI** — the full service-account JSON.`region` is optional and defaults to`"global"` ; set it if your model is region-specific.

`credential_type` or a missing `region` is a frequent cause of silent failures.
## 429 — rate limited on your own provider account

This is
**your provider’s**rate limit, not Mesh’s. Requests-per-minute and token limits are governed by your OpenAI / AWS / Google account.

- Raise the quota in the provider console, or slow down.
- With fallback enabled, Mesh retries on platform credentials — so you may see success *and* a hidden fallback (see below).

## Fallback masks a broken key (or a disabled fallback hard-fails)

By default, if your BYOK key fails, Mesh
**falls back to platform credentials**and the request still succeeds — billed to your Mesh balance, not your provider. A key that’s been broken for weeks can go unnoticed this way.

- To verify a key actually works, temporarily set the key to **Exclusive Use** (fallback disabled) and watch for failures.
- With **Exclusive Use** on, a key failure means the whole request fails — there is no safety net. Expected behavior, not a bug.

## Unexpected BYOK charges

- A **4% platform fee** applies to BYOK requests (the first 1,000,000 tokens are fee-free).
- If fallback fired, those tokens were billed to your **Mesh balance** at platform rates, not your provider account — check Dashboard →**Logs** to see which credential served each request.

## Still stuck?

Check Dashboard →
**Logs**for the exact provider error, then see the

[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/byok
