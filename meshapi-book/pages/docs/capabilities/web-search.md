---
type: Web Page
title: Web Search - Mesh API
description: Search the live web through a single endpoint — engines, failover, filtering,
  and the flat per-search fee.
resource: https://developers.meshapi.ai/docs/capabilities/web-search
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`POST /v1/web/search` returns live web results through one interface, with automatic failover between engines. Use it to ground a model’s answer in current information, or as a standalone search API.
This is distinct from the hosted `web_search_preview` tool on the [Responses API](/docs/capabilities/responses-api), which searches

*inside*a model turn. This endpoint returns results to your code.

## Basic use

### Parameters

## Engines and failover

By default MeshAPI tries the
**native**engine first and falls back to

**Tavily**if it fails — you get a result without handling engine outages yourself.

Pinning 

`provider` **disables failover**. If you pin an engine and it is down, the request fails rather than trying the other one. Omit`provider` unless you specifically need one engine’s behaviour.`search_depth` is a Tavily-only control and is ignored by the native engine.
## Billing

Because it is a flat fee per call rather than per token, a high-volume search workload can cost more than you’d predict from token pricing alone. Budget by call count.
## The `allowed_models` trap

See [API Keys](/docs/getting-started/api-keys)for how allow-lists behave more generally.

## Related

- [Responses API](/docs/capabilities/responses-api) — in-turn hosted web search
- [API Keys](/docs/getting-started/api-keys) — allow-lists and limits
- [Pricing](/docs/getting-started/pricing)

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/web-search
