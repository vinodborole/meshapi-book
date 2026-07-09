---
type: Web Page
title: Search Models | Mesh API Docs
description: Paginated, filterable, fuzzy-searchable model listing — server-side equivalent
  of the dashboard's former client-side table handling.
resource: https://developers.meshapi.ai/api-reference/mesh-api/models/search-models
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Search Models

Paginated, filterable, fuzzy-searchable model listing — server-side equivalent of the dashboard’s former client-side table handling.

Note: there is intentionally no upstream-provider filter — the routing provider (e.g. openrouter) is never exposed on the public API.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

q

Fuzzy search over name, id, and brand

free

Plan filter: true = free only, false = paid only, omit = all

discounted

true = discounted only, false = regular only, omit = all

input_modality

Match models accepting any of these input modalities

output_modality

Match any output modality (incl. ‘embedding’/‘realtime’ capabilities)

brand

Filter by brand (vendor)

sort

Allowed values:

order

Allowed values:

limit

offset

### Response

Successful Response

items

total

limit

offset

brands

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/models/search-models
