---
type: Web Page
title: Chat Completions - Mesh API
description: 'OpenAI-compatible chat completions endpoint. Auth: Authorization: Bearer
  rsk_ Streaming: set stream=true for SSE chunks Templates: set template="name" +
  variables={...} Rate limits: RPM and RPD enforced per key via Redis fixed-window
  counters Spend cap: enforced if key.spend_cap_usd is set (soft cap)'
resource: https://developers.meshapi.ai/api/chat/chat-completions
timestamp: '2026-09-21T12:28:28.555532+00:00'
---

# Chat Completions

#### Authorizations

Enter your MeshAPI key (`rsk_...`) or, for the admin-keys endpoints, a dashboard session token — sent as `Authorization: Bearer <token>`.

#### Headers

Dated version of the API contract to pin this request to. Omit it and the request is served under `2026-08` — the oldest supported version, so an existing integration is never moved by a release. A malformed or unsupported value is rejected with `400 invalid_api_version` rather than falling back silently. The version actually served is echoed as `X-Mesh-Version` on every response, including errors.

`2026-08`, `2026-09` #### Body

`0 <= x <= 2``x >= 1``0 <= x <= 1``-2 <= x <= 2``-2 <= x <= 2``high`, `medium`, `low`, `none` Structured `reasoning` object (supersedes `reasoning_effort`).

`effort` and `max_tokens` are mutually exclusive: effort is the
OpenAI/Grok dial, max_tokens the Anthropic/Gemini/Qwen thinking budget.
`exclude` reasons internally but omits the content from the response.
`enabled: true` alone means "reason at defaults"; `false` disables.
`context`/`mode` are gpt-5.x-only passthroughs.

- Tool
- ServerTool

A stable, anonymised identifier for the end user making this request, recorded on the usage row and available as a filter and a group-by dimension in the usage API. Supersedes the deprecated `user` field. Use an opaque id, not an email address or a name.

`256``256``text`, `image` `text`, `audio`, `image` `x > 0``auto`, `default`, `flex` Your own labels for this request, echoed back on the usage row and available as a filter and a group-by dimension in the usage API. String keys to string values. Never put personal or sensitive data here — tags are stored with the usage record and are not redacted.

# Citations

1. Source page: https://developers.meshapi.ai/api/chat/chat-completions
