---
type: Web Page
title: Structured Output - Mesh API
description: Constrain chat completion output to valid JSON or a strict JSON schema
  with response_format.
resource: https://developers.meshapi.ai/docs/capabilities/structured-output
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`response_format` field on `POST /v1/chat/completions` controls the shape of the model’s output. Use it to get back machine-readable JSON instead of free-form prose — ideal for data extraction, classification, and tool-style workflows.
**Not every model enforces**It only takes effect on models that support structured output. If a model doesn’t, the request still

`response_format`.
**succeeds and returns ordinary text**— it does not error. So if you get prose back instead of the JSON you asked for, the model most likely doesn’t support structured output: check its support on the

[Models page](https://app.meshapi.ai)in your dashboard (

**app.meshapi.ai → Models**, i.e.

`https://app.meshapi.ai/org/<your-org-id>/models`) — or the `supports_structured_output` flag returned by `GET /v1/models` — and switch to one with first-class support such as OpenAI models or Google Gemini (e.g. `google/gemini-2.5-flash`). See [Not getting JSON back?](#not-getting-json-back)and

[How it works across providers](#how-it-works-across-providers).

## Output modes

`response_format` is an object whose `type` selects the mode:
In every mode the JSON is returned as a 

**string**in

`choices[0].message.content`, which you parse client-side.
## Schema-enforced output (`json_schema`)

Supply a `json_schema` object containing a `name` (a label) and a `schema` (the JSON Schema to enforce):
- curl
- Python SDK
- Node.js SDK

## Valid JSON without a schema (`json_object`)

When you only need parseable JSON and don’t want to define a schema, use `json_object` and describe the keys you expect in the prompt:
`json_schema` when you need that guarantee.
## How it works across providers

`response_format` follows the OpenAI convention and is forwarded to the upstream provider. For providers with a different native contract, MeshAPI translates it automatically — for example, Google Gemini models on Vertex AI are converted to Gemini’s native structured-output config (`responseMimeType` for `json_object`, plus `responseSchema` for `json_schema`), so schema enforcement runs on the provider side.
Structured output works with any model that supports `response_format`. If a specific model doesn’t, it simply returns ordinary text.
## Tips

- Set `additionalProperties: false` in your schema to forbid extra fields.
- Use `temperature: 0` for the most deterministic, schema-faithful output.
- The content is a JSON string — parse it with `json.loads()` /`JSON.parse()` .
- On success `finish_reason` is`"stop"` .

## Not getting JSON back?

If the response comes back as ordinary prose instead of the JSON you asked for, the model you chose doesn’t support structured output.`response_format` is forwarded to the provider, but a provider that doesn’t support it just returns plain text — the request still succeeds with `finish_reason: "stop"`, so this fails **silently**rather than erroring.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/structured-output
