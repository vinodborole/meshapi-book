---
type: Web Page
title: List models (public) | Mesh API Docs
description: Public, unauthenticated model catalog for the marketing site. Returns
  the same enabled-model listing as GET /v1/models — model id,
resource: https://developers.meshapi.ai/api-reference/mesh-api/models/list-public-models
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# List models (public)

List models (public)

### Query parameters

Filter: true = free models only, false = paid only, omit = all

Filter by model_type: text, embedding, image, audio, video

Public, unauthenticated model catalog for the marketing site.

Returns the same enabled-model listing as `GET /v1/models` — model id,
brand, context window, per-1M pricing, cache pricing, modalities and
capability flags — but with **no auth and no per-user discount enrichment**.
Only public-safe fields are exposed: upstream provider costs are never part
of `ModelOut`, and no discount/owner data is read.

Cached at two layers: the catalog is served from a shared Redis cache
(`_get_models`), and the HTTP response carries a public `Cache-Control`
header so browsers and any CDN in front of the API can cache it too. Both
windows track the same TTL, so a stale entry lives at most one TTL past an
admin write (which invalidates the Redis cache immediately).

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/models/list-public-models
