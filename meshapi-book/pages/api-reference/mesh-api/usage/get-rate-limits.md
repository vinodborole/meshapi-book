---
type: Web Page
title: Get Rate Limits | Mesh API Docs
description: Return live rate-limit usage (RPM / RPD / TPM) vs limits. Always reads
  fresh Redis counters — no caching.
resource: https://developers.meshapi.ai/api-reference/mesh-api/usage/get-rate-limits
timestamp: '2026-07-27T10:02:52.764636+00:00'
---

# Get Rate Limits

Return live rate-limit usage (RPM / RPD / TPM) vs limits. Always reads fresh Redis counters — no caching.

Authenticate with an rsk_ API key; the response returns a single `key` scope
for that key (org / team / member scopes are null).

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

org_id

Ignored for API-key callers — usage is always scoped to the authenticated key.

team_id

Team UUID (optional filter)

### Response

Successful Response

as_of

org

Rate limits for an org / team / member scope.

team

Rate limits for an org / team / member scope.

member

Rate limits for an org / team / member scope.

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/usage/get-rate-limits
