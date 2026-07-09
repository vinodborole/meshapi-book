---
type: Web Page
title: Structured Output | Mesh API Docs
description: Constrain chat completion output to valid JSON or a strict JSON schema
  with response_format.
resource: https://developers.meshapi.ai/docs/guides/structured-output
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

# Structured Output

The `response_format` field on `POST /v1/chat/completions` controls the shape of the model’s output. Use it to get back machine-readable JSON instead of free-form prose — ideal for data extraction, classification, and tool-style workflows.

**Not every model enforces  response_format.** It only takes effect on models that support structured output. If a model doesn’t, the request still 

**succeeds and returns ordinary text**— it does not error. Always parse/validate the response, and prefer a model with first-class support such as OpenAI models or Google Gemini (e.g.

`google/gemini-2.5-flash`). See [How it works across providers](/docs/guides/structured-output#how-it-works-across-providers).

## Output modes

`response_format` is an object whose `type` selects the mode:

In every mode the JSON is returned as a **string** in `choices[0].message.content`, which you parse client-side.

## Schema-enforced output (`json_schema`)

Supply a `json_schema` object containing a `name` (a label) and a `schema` (the JSON Schema to enforce):

###### curl

###### Python SDK

###### Node.js SDK

## Valid JSON without a schema (`json_object`)

When you only need parseable JSON and don’t want to define a schema, use `json_object` and describe the keys you expect in the prompt:

The output is guaranteed to be valid JSON, but the exact keys depend on the model following your prompt. Use `json_schema` when you need that guarantee.

## How it works across providers

`response_format` follows the OpenAI convention and is forwarded to the upstream provider. For providers with a different native contract, MeshAPI translates it automatically — for example, Google Gemini models on Vertex AI are converted to Gemini’s native structured-output config (`responseMimeType` for `json_object`, plus `responseSchema` for `json_schema`), so schema enforcement runs on the provider side.

Structured output works with any model that supports `response_format`. If a specific model doesn’t, it simply returns ordinary text.

## Tips

- Set `additionalProperties: false`in your schema to forbid extra fields.
- Use `temperature: 0`for the most deterministic, schema-faithful output.
- The content is a JSON string — parse it with `json.loads()`/`JSON.parse()`.
- On success `finish_reason`is`"stop"`.

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/structured-output
