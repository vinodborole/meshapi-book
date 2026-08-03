---
type: Web Page
title: Web Search | Mesh API Docs
description: Search the live web through a single endpoint — engines, failover, filtering,
  and the flat per-search fee.
resource: https://developers.meshapi.ai/docs/guides/web-search
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Web Search

`POST /v1/web/search` returns live web results through one interface, with automatic failover between engines. Use it to ground a model’s answer in current information, or as a standalone search API.

This is distinct from the hosted `web_search_preview` tool on the [Responses API](/docs/guides/responses-api), which searches *inside* a model turn. This endpoint returns results to your code.

## Basic use

### Parameters

## Engines and failover

By default MeshAPI tries the **native** engine first and falls back to **Tavily** if it fails — you get a result without handling engine outages yourself.

Pinning `provider` **disables failover**. If you pin an engine and it is down, the request fails rather than trying the other one. Omit `provider` unless you specifically need one engine’s behaviour.

`search_depth` is a Tavily-only control and is ignored by the native engine.

## Billing

Web search is billed a **flat $0.005 per successful search**, regardless of engine, result count, or `search_depth`. It is not token-priced — a 1-result search and a 20-result search cost the same.

Searches that fail or return nothing are **not** charged.

Because it is a flat fee per call rather than per token, a high-volume search workload can cost more than you’d predict from token pricing alone. Budget by call count.

## The `allowed_models` trap

If your API key has an `allowed_models` allow-list, web search performs its pre-flight check against its own model pin. **An allow-list that omits that model silently disables web search for the key.**

The failure is not obviously about the allow-list. If web search stops working right after you restrict a key, this is the cause — add the search model to the list, or leave the key unrestricted.

See [API Keys](/docs/guides/api-keys) for how allow-lists behave more generally.

## Related

- [Responses API](/docs/guides/responses-api) — in-turn hosted web search
- [API Keys](/docs/guides/api-keys) — allow-lists and limits
- [Pricing](/docs/introduction/pricing)

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/web-search
