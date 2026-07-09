---
type: Web Page
title: Model Explanation | Mesh API Docs
description: Understand the different types of AI models available through the API.
resource: https://developers.meshapi.ai/docs/introduction/model-explanation
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Model Explanation

# Understanding Models

MeshAPI acts as a router, forwarding your standardized API calls to an expansive list of underlying foundational models. A **Model** represents a distinct neural network trained by various AI organizations (like OpenAI, Google, Anthropic, Meta, etc.).

## Base Models

Base models are the standard foundational engines you chat with. They take a series of messages and output a sequence of text.

### Naming Convention

Model names typically follow a prefix structure: `<provider>/<model_name>`.
For example:

- `openai/gpt-4o-mini`
- `anthropic/claude-haiku-4.5`

## Capabilities & Context Limits

Each model handles a different maximum “context limit” – the number of tokens (words/characters) it can process in a single request.

- Fast, small models (e.g. `llama-3`) may have smaller limits but respond instantly.
- Large models (e.g. `gpt-4o`) can handle massive documents and are heavily optimized for reasoning but take slightly longer to generate tokens.

## Choosing the Right Model

When deciding what model to use for your application, consider these factors:

- **Cost:**Do you need high intelligence, or just rapid categorization?
- **Speed (Latency):**Lighter models offer much lower time-to-first-token.
- **Context Length:**If you are passing an entire codebase or large PDF, ensure the model supports large contexts.

You can view the full dynamic list of supported models in our live Model Catalog.

## Discovering models via the API

### Model object fields

Each model includes `id`, `name`, `model_type`, `input_modalities`, `output_modalities`, `is_free`, and a `pricing` object (`prompt_usd_per_1k`, `completion_usd_per_1k`, and per-million-token variants).

Capability is exposed via `supports_*` boolean flags, including `supports_thinking`, `supports_tools`, `supports_structured_output`, `supports_system_prompt` (defaults to `true`), `supports_completions_api`, `supports_responses_api`, `supports_realtime`, `supports_embeddings`, `supports_batching`, `supports_video_generation`, and the `supports_image_*` edit flags. Check these before sending a request that relies on a specific capability.

# Citations

1. Source page: https://developers.meshapi.ai/docs/introduction/model-explanation
