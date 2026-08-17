---
type: Web Page
title: Chat Completions - Mesh API
description: 'OpenAI-compatible chat completions endpoint. Auth: Authorization: Bearer
  rsk_ Streaming: set stream=true for SSE chunks Templates: set template="name" +
  variables={...} Rate limits: RPM and RPD enforced per key via Redis fixed-window
  counters Spend cap: enforced if key.spend_cap_usd is set (soft cap)'
resource: https://developers.meshapi.ai/api/chat/chat-completions
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

Chat Completions

Chat

# Chat Completions

OpenAI-compatible chat completions endpoint.
Auth:        Authorization: Bearer rsk_<ULID>
Streaming:   set stream=true for SSE chunks
Templates:   set template="name" + variables={...}
Rate limits: RPM and RPD enforced per key via Redis fixed-window counters
Spend cap:   enforced if key.spend_cap_usd is set (soft cap)

POST

Chat Completions

#### Authorizations

Enter your MeshAPI key (`rsk_...`) — sent as `Authorization: Bearer <key>`.

#### Headers

Dated version of the API contract to pin this request to. Omit it and the request is served under `2026-08` — the oldest supported version, so an existing integration is never moved by a release. A malformed or unsupported value is rejected with `400 invalid_api_version` rather than falling back silently. The version actually served is echoed as `X-Mesh-Version` on every response, including errors.

Available options: 

`2026-08` #### Body

application/json

Required range: 

`0 <= x <= 2`
Required range: 

`x >= 1`
Required range: 

`0 <= x <= 1`
Required range: 

`-2 <= x <= 2`
Required range: 

`-2 <= x <= 2`
Available options: 

`high`, `medium`, `low`, `none` Maximum string length: 

`256`
Available options: 

`text`, `image` Available options: 

`text`, `audio`, `image` Required range: 

`x > 0`

# Citations

1. Source page: https://developers.meshapi.ai/api/chat/chat-completions
