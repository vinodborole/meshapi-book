---
type: Web Page
title: Create Response | Mesh API Docs
description: 'Responses API endpoint — provider resolved dynamically from DB. Auth:
  Authorization: Bearer rsk<ULID> Streaming: set stream=true for SSE chunks'
resource: https://developers.meshapi.ai/api-reference/mesh-api/responses/create-response
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Response

Responses API endpoint — provider resolved dynamically from DB.

Auth: Authorization: Bearer rsk_<ULID> Streaming: set stream=true for SSE chunks Rate limits: RPM and RPD enforced per key via Redis fixed-window counters Spend cap: enforced if key.spend_cap_usd is set Provider: resolved from model_pricing.provider (same as chat/completions)

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

input

model

template

variables

session_id

stream

max_output_tokens

temperature

top_p

seed

reasoning

tools

tool_choice

response_format

plugins

user

previous_response_id

instructions

thinking

caching

store

background

include

expire_at

max_tool_calls

context_management

text

timeout

### Response

Completed response (JSON), a queued background job when background=true, or an SSE stream when stream=true

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/responses/create-response
