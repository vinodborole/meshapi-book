---
type: Web Page
title: Get Balance | Mesh API Docs
description: Return the credit balance for the authenticated caller. Authenticate
  with an rsk API key. If the key belongs to an organization, the
resource: https://developers.meshapi.ai/api-reference/mesh-api/balance/get-balance
timestamp: '2026-07-27T10:02:52.764636+00:00'
---

# Get Balance

Return the credit balance for the authenticated caller.
Authenticate with an rsk_ API key. If the key belongs to an organization, the
organization's shared balance is returned — all members of an org share the
same billing pool; a key with no org gets its own balance. The response also
reports how much of the balance is reserved by in-flight operations
(`reserved_usd`, plus a per-category `reserved_breakdown`) and the spendable
remainder (`available_usd`).

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Response

Successful Response

balance_usd

Total credit balance in USD.

available_usd

Spendable balance: balance_usd minus reserved_usd.

reserved_usd

Funds currently held by in-flight operations and not spendable.

updated_at

Last balance update (ISO 8601).

message

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/balance/get-balance
