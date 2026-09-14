---
type: Web Page
title: Chat Compare - Mesh API
description: Send one conversation to several models at once and get their answers
  together. Pass 2–10 model ids in models with the usual messages. They run in parallel
  and each result carries its own content, token usage and latency, so you can compare
  quality against cost on real traffic instead of guessing. Set comparison_model to
  have a further model judge the answers, optionally steered by comparison_instructions.
  Use model_overrides when one model needs different parameters from the rest; temperature
  and max_tokens otherwise apply to all. With stream true, results arrive as they
  finish rather than after the slowest. Every model in the list is a billed call,
  so a comparison across five models costs five completions plus the judge, and one
  model failing does not fail the request — that result reports its own error.
resource: https://developers.meshapi.ai/api/chat/chat-compare
timestamp: '2026-09-14T12:21:17.301704+00:00'
---

# Chat Compare

Send one conversation to several models at once and get their answers together.

Pass 2–10 model ids in `models` with the usual `messages`. They run in parallel and
each result carries its own content, token usage and latency, so you can compare
quality against cost on real traffic instead of guessing.

Set `comparison_model` to have a further model judge the answers, optionally steered
by `comparison_instructions`. Use `model_overrides` when one model needs different
parameters from the rest; `temperature` and `max_tokens` otherwise apply to all.
With `stream` true, results arrive as they finish rather than after the slowest.

**Every model in the list is a billed call**, so a comparison across five models
costs five completions plus the judge, and one model failing does not fail the
request — that result reports its own error.

#### Authorizations

Enter your MeshAPI key (`rsk_...`) or, for the admin-keys endpoints, a dashboard session token — sent as `Authorization: Bearer <token>`.

#### Headers

Dated version of the API contract to pin this request to. Omit it and the request is served under `2026-08` — the oldest supported version, so an existing integration is never moved by a release. A malformed or unsupported value is rejected with `400 invalid_api_version` rather than falling back silently. The version actually served is echoed as `X-Mesh-Version` on every response, including errors.

`2026-08`, `2026-09` #### Body

`1 - 10` elements`0 <= x <= 2``x >= 1`
Your own labels for this request, echoed back on the usage row and available as a filter and a group-by dimension in the usage API. String keys to string values. Never put personal or sensitive data here — tags are stored with the usage record and are not redacted.

A stable, anonymised identifier for the end user making this request, recorded on the usage row and available as a filter and a group-by dimension in the usage API. Supersedes the deprecated `user` field. Use an opaque id, not an email address or a name.

`256`
#### Response

Per-model results plus an optional synthesized comparison (JSON), or an SSE stream when stream=true

Unique ID for this comparison (`cmp_...`).

Unix timestamp (seconds) when the response was produced.

Models that were compared, in request order (deduped).

Per-model results, in `models` order.

End-to-end latency for the whole compare request, in milliseconds.

`"compare.completion"`
Synthesized evaluation of all responses from the comparison LLM. Null when `skip_comparison` is true or fewer than two models succeeded.

Model that produced `comparison`. Null when no synthesis ran.

Token usage for the comparison LLM call.

True if the primary comparison model failed and a fallback produced the synthesis.

True if at least one model in `results` returned an error.

Echoes whether the comparison LLM step was skipped.

# Citations

1. Source page: https://developers.meshapi.ai/api/chat/chat-compare
