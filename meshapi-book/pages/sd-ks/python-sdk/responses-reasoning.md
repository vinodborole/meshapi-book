---
type: Web Page
title: Responses API (Reasoning) | Mesh API Docs
description: Query reasoning models with the Python SDK.
resource: https://developers.meshapi.ai/sd-ks/python-sdk/responses-reasoning
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Responses API (Reasoning)

Responses API (Reasoning)

# Responses API (Reasoning Models)

Use the responses API to query o-series and similar models that provide reasoning efforts.

Streaming works the same way via `client.responses.stream(params)`.

Use `client.responses.list()` and `client.responses.get("resp_...")` for persisted/background response jobs. A response returned from a synchronous `create` call is not guaranteed to be retrievable later by ID.

# Citations

1. Source page: https://developers.meshapi.ai/sd-ks/python-sdk/responses-reasoning
