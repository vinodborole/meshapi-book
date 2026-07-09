---
type: Web Page
title: Embeddings | Mesh API Docs
description: Model support, dimensions, batch ordering, and input-size limits.
resource: https://developers.meshapi.ai/debug/embeddings
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Embeddings

Model support, dimensions, batch ordering, and input-size limits.

Most embeddings issues are request-shape or model-capability mismatches. See Embeddings for the full reference.

## 422 — model missing or unsupported field

- `model`is required unless your key has a default model configured.
- `dimensions`only works on models that- **support truncation**— sending it to a model that doesn’t will be rejected.
- `input_type`,- `instructions`, and- `sparse_embedding`are provider-specific (e.g. asymmetric models, BytePlus) — a model that doesn’t support them may reject the request.

## Results seem mismatched to my inputs

When you pass an array of strings, the returned `data` array is in the **same
order** as your input. Match by `index`, and don’t assume the API reorders or
dedupes — it doesn’t.

## Input too long

Each model has a `context_length`. Inputs longer than that are rejected — check
the model’s `context_length` via `GET /v1/models` and chunk long text before
embedding. *(Behavior inferred from the documented  context_length field.)*

## Garbled vectors / wrong format

`encoding_format` controls the output: `"float"` returns a numeric array,
`"base64"` returns a packed string you must decode. If your vectors look like
gibberish, you’re probably reading `base64` output as floats.

## Still stuck?

See the Mesh API error reference
or email **contact@meshapi.ai**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/embeddings
