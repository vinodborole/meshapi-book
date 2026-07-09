---
type: Web Page
title: Chat Completions | Mesh API Docs
description: 'OpenAI-compatible chat completions endpoint. Auth: Authorization: Bearer
  rsk<ULID> Streaming: set stream=true for SSE chunks'
resource: https://developers.meshapi.ai/api-reference/mesh-api/chat/completions
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Chat Completions

OpenAI-compatible chat completions endpoint.

Auth: Authorization: Bearer rsk_<ULID> Streaming: set stream=true for SSE chunks Templates: set template=“name” + variables={…} Rate limits: RPM and RPD enforced per key via Redis fixed-window counters Spend cap: enforced if key.spend_cap_usd is set (soft cap)

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

messages

model

template

variables

session_id

stream

temperature

max_tokens

top_p

frequency_penalty

presence_penalty

stop

seed

reasoning_effort

tools

tool_choice

response_format

transforms

models

user

modality

image

async_mode

modalities

audio

timeout

cache

### Response

Completion (JSON), or an SSE stream when stream=true

id

object

created

model

choices

usage

system_fingerprint

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/chat/completions
