---
type: Web Page
title: Dashboard - Mesh API
description: A guided tour of the Mesh API control panel.
resource: https://developers.meshapi.ai/docs/getting-started/dashboard
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

[Mesh Dashboard](https://app.meshapi.ai)is your control center for managing API keys, billing, usage logs, and prompt templates. This page walks through each section.

## API Keys

Create and manage your`rsk_...` credentials.
- **Create keys** with descriptive labels (e.g.`production` ,`staging` ,`teammate-alice` )
- **Set spend caps** — a hard USD limit per key to prevent runaway costs
- **Configure rate limits** — Requests Per Minute (RPM), Requests Per Day (RPD), and Tokens Per Minute (TPM)
- **Monitor per-key metrics** — request counts, success/error rates, and total spend

### Key creation options

When creating a key you can configure global limits that apply to all models, and optionally restrict which models the key can use with per-model overrides:
**Global limits (apply across all models on this key):**

**Model allowlist (optional):**Select specific models this key is permitted to use. If no models are selected, the key can access all models available to your account.

**Per-model limits (optional):**For each model added to the allowlist you can set independent rate limits and a spend cap that apply only when that model is called:

Per-model limits are enforced in addition to the global key limits — whichever limit is hit first takes effect.

## Billing

Manage your pre-paid credit balance.
- **Current balance** — real-time view of remaining credits in USD
- **Top up** — add credits via Stripe or local payment gateways (UPI, cards)
- **Spend history** — detailed breakdown by provider, model, and date range
- **Auto-recharge** — automatically top up when your balance drops below a threshold

## Logs

Every request through your account is recorded.
- **Filter** by date, model, API key, or status (success / error)
- **Latency tracking** — time-to-first-token and total response time per request
- **Token counts** — prompt and completion token breakdown per call
- **Request ID** — a unique`req_...` ID on every request, useful for support tickets

## Models

Browse the full model catalog.
- **Discover** all enabled models across OpenAI, Anthropic, Google, Meta, and more
- **Compare pricing** — live prompt and completion cost per 1M tokens
- **Filter free models** — find $0-cost models for testing and prototyping
- **Check capabilities** — context length, supported modalities (text, image, audio)

## Templates

Create and manage
[Prompt Templates](/docs/capabilities/prompt-templates)through a visual editor.

- **Write system prompts** with`{{variable}}` slots for dynamic values
- **Set default parameters** — temperature, max tokens, stop sequences
- **Test in-dashboard** before deploying to production
- **Manage versions** — update templates without touching client code

## Usage Analytics

Aggregate views of your AI spend and traffic patterns.
- **By model** — see which models consume the most tokens and cost
- **By key** — identify which integrations are driving the most traffic
- **Time-series** — visualize request volume and latency trends over time
- **Error analysis** — track upstream provider error rates

## Provider Keys (BYOK)

Configure your own upstream API keys for providers like AWS Bedrock, Google Vertex AI, and OpenAI. See
[Bring Your Own Keys](/docs/capabilities/byok).

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/dashboard
