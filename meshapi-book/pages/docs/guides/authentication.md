---
type: Web Page
title: Authentication | Mesh API Docs
description: Securely authenticate your requests to the Mesh API.
resource: https://developers.meshapi.ai/docs/guides/authentication
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Authentication

Authentication with the Mesh API is simple and secure. It uses standard HTTP `Authorization` headers with a unique **Router Service Key (RSK)**.

## Key Types

Mesh supports two types of keys, each found in different parts of your developer dashboard.

### 1. Account Keys (RSK)

Account-level keys are your primary method of interacting with the API. They are prefixed with `rsk_`.

- **Scope** : Access to all inference, discovery, and management endpoints.
- **Spend Caps** : Can be configured with monthly or total spend limits to prevent cost overruns.
- **Rate Limits** : Configurable Requests Per Minute (RPM), Requests Per Day (RPD), and Tokens Per Minute (TPM).
- **Default Model** : Optionally set a default model used when a request doesn’t specify one.

### 2. Provider Keys (Internal)

If you provide your own API keys for upstream providers (OpenAI, Anthropic, etc.), Mesh handles the secure storage and rotation of these keys via GCP Secret Manager. You generally don’t use these keys directly in your code; Mesh uses them to fulfill requests on your behalf.

## Security Best Practices

Your RSK keys are treated as secrets. Treat them with the same care as your database credentials.

1. **Keep Keys Secret** : Never expose your`rsk_` keys in client-side code (browsers or mobile apps). Always use a backend proxy.
2. **Use Spend Caps** : Always configure a spend cap for each key to limit potential losses if a key is compromised.
3. **Rotate Keys** : If you suspect a key has been leaked, immediately deactivate it and create a new one in the**API Keys** section of the dashboard.
4. **Environment Variables** : Store your keys as environment variables or in a secure secret manager, never hard-coded in your source files.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/authentication
