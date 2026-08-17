---
type: Web Page
title: Claude Code - Mesh API
description: Route Claude Code through Mesh to reach every model in the catalog, with
  your spend caps, rate limits and usage logging applied.
resource: https://developers.meshapi.ai/docs/capabilities/claude-code
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

[Anthropic Messages API](/docs/capabilities/messages-api)to whatever

`ANTHROPIC_BASE_URL` points at. Point it at Mesh and every request runs
through the same gateway as the rest of your traffic — spend caps, per-key rate
limits, usage logging and retry/fallback all apply, with no change to how you use
the tool.
Because 

`/v1/messages` is **not restricted to Anthropic models**, this also lets Claude Code run against any model in the Mesh catalog.
## Configuration

Add this to`~/.claude/settings.json` (or a project’s `.claude/settings.json`):
## The three things that catch people out

### 1. If you are signed in to Claude Code, `ANTHROPIC_AUTH_TOKEN` is ignored

This is the most common failure, and the error does not hint at the cause:
### 2. `ANTHROPIC_DEFAULT_OPUS_MODEL` is not optional

Claude Code’s default model is Opus. Set only Sonnet and Haiku and the first
request fails:
`[1m]` suffix Claude Code appends for its long-context variant — another
reason to map the variable explicitly rather than relying on a default.
### 3. Haiku carries real traffic

`ANTHROPIC_DEFAULT_HAIKU_MODEL` is easy to skip because you never select Haiku
yourself. Claude Code uses it for background work — conversation titles,
summaries — so leaving it unset sends a steady trickle of requests to an id Mesh
does not serve. In a short verification session, roughly a third of the requests
were Haiku.
## Verify it works

- Dashboard
- API

Open 

[Usage](https://app.meshapi.ai/usage). Requests from Claude Code appear with endpoint**.**`messages`
## Model discovery (optional)

Claude Code can populate its`/model` picker from the Mesh catalog instead of its
built-in list:
- Discovery calls `GET /v1/models` , which accepts**`Authorization: Bearer` only** —
not`x-api-key` . With`ANTHROPIC_AUTH_TOKEN` (as configured above) this is
already correct; if you switch to`ANTHROPIC_API_KEY` , inference keeps working
but discovery returns 401.
- The catalog is large — the response covers the full model list and is fetched on start-up.

## What runs through the gateway

Everything, which is the point of routing Claude Code this way:
## Troubleshooting

## Related

- [Messages API](/docs/capabilities/messages-api) — the endpoint Claude Code uses
- [Available Models](/docs/reference/models-list) — exact ids for the model variables
- [Rate Limits](/docs/getting-started/rate-limits) — what applies to a session
- [Retry & Fallback](/docs/platform/retry-and-fallback) — behaviour on provider failure

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/claude-code
