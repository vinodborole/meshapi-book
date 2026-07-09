---
type: Web Page
title: Dashboard Guide | Mesh API Docs
description: A guided tour of the Mesh API Control Panel.
resource: https://developers.meshapi.ai/docs/guides/dashboard-guide
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Dashboard Guide

The Mesh Dashboard is your central control center for managing AI models, keys, and billing. This guide walks through each section to help you make the most of the service.

## 1. API Keys

The **API Keys** section allows you to manage multiple sets of credentials. You can:

- **Create Keys**: Generate new- `rsk_`tokens with optional labels.
- **Set Spend Caps**: Define a maximum USD limit for each key.
- **Rate Limits**: Configure per-key requests-per-minute (RPM), requests-per-day (RPD), and tokens-per-minute (TPM) limits.
- **Monitor Usage**: See total requests, successful calls, and error rates per key.
- **Update Defaults**: Set a default model (e.g.,- `openai/gpt-5.4-mini`) for when one is not specified in the request.

## 2. Billing

Mesh provides a unified balance across all providers.

- **Current Balance**: Real-time view of your remaining pre-paid credits.
- **Top-up**: Add credits via Stripe or local payment gateways.
- **Spend History**: Detailed breakdown of costs per provider and per model.
- **Auto-recharge**: Automatically top up your balance when it falls below a threshold you set.

## 3. Logs

The **Logs** section provides a historical record of every request made through your account.

- **Usage Events**: Filter logs by date range, model, or status (success/error).
- **Latency Tracking**: monitor the time taken by upstream providers.
- **Token Count**: precise breakdowns of prompt and completion tokens for every call.
- **Request ID**: Every request has a unique- `req_...`ID to simplify debugging with the Mesh support team.

## 4. Models

Explore all available models and their current Pricing.

- **Discovery**: See which models are currently enabled (OpenAI, Anthropic, Gemini, Haiku, etc.).
- **Pricing**: live view of pricing per 1,000 tokens for both prompt and completion.
- **Free Models**: Easily filter for models with $0 cost (ideal for testing).

## 5. Templates

Manage your prompts on the server to ensure consistency.

- **In-Dashboard Editor**: Write and test your system and user messages.
- **Parameter Tuning**: Set default temperature, max tokens, and stop sequences.
- **Variables**: Use- `{{variable}}`syntax and pass values in the API request to dynamically render prompts.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/dashboard-guide
