---
type: Web Page
title: Chat Completions - Mesh API
description: 'OpenAI-compatible chat completions endpoint. Auth: Authorization: Bearer
  rsk_ Streaming: set stream=true for SSE chunks Templates: set template="name" +
  variables={...} Rate limits: RPM and RPD enforced per key via Redis fixed-window
  counters Spend cap: enforced if key.spend_cap_usd is set (soft cap)'
resource: https://developers.meshapi.ai/api/chat/chat-completions
timestamp: '2026-08-10T07:50:31.317333+00:00'
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

#### Body

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
