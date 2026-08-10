---
type: Web Page
title: Structured Output - Mesh API
description: Constrain model output to a JSON schema for reliable structured data
  extraction.
resource: https://developers.meshapi.ai/sdk/structured-output
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`response_format` with `type: "json_schema"` to get a response that always matches your schema. Works with any model that supports `response_format`, including OpenAI and Google Gemini models (e.g. `google/gemini-2.5-flash`).
## Output modes

`response_format` is an object whose `type` selects how the output is shaped:
In the JSON modes the content comes back as a 

**string**in

`choices[0].message.content` — parse it client-side.
## Basic example

- Python
- Node.js
- Go

## Valid JSON without a schema

When you only need parseable JSON and don’t want to define a schema, use`json_object` and describe the expected keys in the prompt:
`json_schema` when you need that guarantee.
## How it works across providers

`response_format` follows the OpenAI convention and is forwarded to the upstream provider. For providers with a different native contract, Mesh API translates it automatically — for example, Google Gemini models on Vertex AI are converted to Gemini’s native structured-output config (`responseMimeType` for `json_object`, plus `responseSchema` for `json_schema`), so enforcement runs on the provider side.
## Supported models

Structured output works with any model that supports`response_format` — this includes OpenAI and Google Gemini models (e.g. `google/gemini-2.5-flash`). Models that don’t support it simply return ordinary text. Use `GET /v1/models` to see the models enabled on your account.
## Notes

- Set `additionalProperties: false` to prevent extra fields in the response.
- `finish_reason` will be`"stop"` on success.
- The response content is a JSON string — parse it with `json.loads()` /`JSON.parse()` /`json.Unmarshal` .

## Auto-retry on validation failure (Python)

Some models only best-effort the schema. Set`max_retries` on `parse()` to feed a
failed response back to the model with the validation error appended. Each retry is
a billed call; the default is `0` (no retry).
`parse()` returns the parsed object directly:
`parse()` is non-streaming — use `create()` when you need the raw string content plus
`usage` and cost metadata. The async client exposes the same `await client.chat.completions.parse(...)`.

# Citations

1. Source page: https://developers.meshapi.ai/sdk/structured-output
