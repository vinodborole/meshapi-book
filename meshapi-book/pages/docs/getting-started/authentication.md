---
type: Web Page
title: Authentication - Mesh API
description: Securely authenticate your requests to the Mesh API.
resource: https://developers.meshapi.ai/docs/getting-started/authentication
timestamp: '2026-08-31T13:14:57.224524+00:00'
---

`Authorization` headers with a **Router Service Key (RSK)**. All requests must be made over HTTPS.

## Key Types

### Router Service Keys (`rsk_...`)

Your primary method of interacting with the API. These are the keys you create in the dashboard.
**What they control:**

- Access to all inference endpoints (`/v1/chat/completions` ,`/v1/embeddings` , etc.)
- Per-key **spend caps** — set a maximum USD limit to prevent cost overruns
- Per-key **rate limits** — configurable Requests Per Minute (RPM), Requests Per Day (RPD), and Tokens Per Minute (TPM)
- Optional **default model** — a fallback model used when none is specified in the request

### Admin Keys (`mak_...`)

The credential for **managing**your organisation from a script — creating and updating API keys, reading resolved limits, configuring alerts. Create one in

**Dashboard → Admin Keys**; the plaintext is shown once.

An admin key carries an explicit permission set (

`keys:read`, `keys:write`, `org:read`, `limits:read`, `limits:write`, `alerts:read`, `alerts:write`) and a scope (`self`, `team`, or `org`) — you can grant only what you hold yourself. Rotating one mints a successor and leaves the predecessor valid for a short grace window; revoking takes effect immediately, with no grace. See [Admin Keys](/docs/getting-started/admin-keys)for the full reference.

Inviting members, changing roles, and transferring ownership are 

**not**on the admin-key surface. Those are dashboard actions performed by a signed-in person, by design.
### Provider Keys (BYOK)

If you supply your own API keys for upstream providers (OpenAI, Anthropic, AWS Bedrock, Google Vertex AI), Mesh securely stores and uses them on your behalf. You never reference these directly in your API calls — Mesh handles routing transparently. See
[Bring Your Own Keys](/docs/capabilities/byok)for setup instructions.

## Security Best Practices

**1. Set spend caps**on every key. This limits your blast radius if a key is compromised — the attacker can only spend up to your cap.

**2. Use environment variables**— never hard-code keys in source files.

**3. Rotate immediately**if you suspect a leak. Deactivate the compromised key in the dashboard and generate a new one. Old keys are invalidated instantly.

**4. Use separate keys per environment**— one for development, one for staging, one for production. This gives you clean audit trails and granular rate limiting.

**5. Monitor per-key usage**in the dashboard

**Logs**section. Unexpected spikes often signal misuse before your spend cap triggers.

## Using the key

All examples in this documentation use`rsk_YOUR_KEY` as a placeholder. Replace it with your actual key:
- curl
- Python
- Node.js

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/authentication
