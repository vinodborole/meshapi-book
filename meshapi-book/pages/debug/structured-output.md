---
type: Web Page
title: Structured Output - Mesh API
description: Fix schema rejections, malformed JSON, and models that ignore response_format.
resource: https://developers.meshapi.ai/debug/structured-output
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**model**honoring

`response_format`. The most
common surprise is that a model silently ignores it. See
[Structured Output](/docs/capabilities/structured-output)for the full guide.

## I get plain text instead of JSON (no error)

**Not every model enforces**On a model that doesn’t support it, the request still

`response_format`.
**succeeds and returns ordinary text**— it does not error.

- Switch to a model with first-class support, e.g. OpenAI models or `google/gemini-2.5-flash` .
- Always parse defensively and handle the case where the content isn’t valid JSON.

## 422 — malformed response_format

The`response_format` object is invalid. Check:
- `type` is one of`"text"` ,`"json_object"` ,`"json_schema"` .
- For `json_schema` , the`json_schema` object has both a`name` and a`schema` .
- The `schema` is valid JSON Schema (no trailing commas, correct types).

## Valid JSON but the wrong keys

`json_object` only guarantees **syntactically valid JSON**— not which keys appear. The structure follows your prompt, not a contract.

- Describe the exact keys you expect in the prompt, **or**
- Use `json_schema` for a guaranteed shape. Set`additionalProperties: false` to forbid extra fields, and list every required field in`required` .

## JSON is truncated / won’t parse

If the output is cut off mid-object,`max_tokens` was too low — the model ran
out of room before closing the JSON.
- Raise `max_tokens` .
- Check `finish_reason` :`"length"` means truncated,`"stop"` means complete.

## content is a string, not an object

The JSON is returned as a
**string**in

`choices[0].message.content`. You must
parse it yourself:
## Output isn’t deterministic across runs

Sampling introduces variation even with a schema.
- Set `temperature: 0` for the most deterministic, schema-faithful output.
- Note that for Gemini models, `response_format` is translated server-side to Gemini’s native`responseMimeType` /`responseSchema` , so enforcement happens on the provider — behavior can differ subtly from OpenAI.

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/structured-output
