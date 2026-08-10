---
type: Web Page
title: Support - Mesh API
description: Get help with Mesh API — debugging tips, contact options, and useful
  resources.
resource: https://developers.meshapi.ai/docs/reference/support
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

## Self-service resources

Before reaching out, these resources often resolve issues quickly:
- **Request logs** — every request has a unique`req_...` ID visible in Dashboard →**Logs** . Filter by date, model, or key to find a specific call.
- **Error codes** — check the`error.code` field in the response body. Common codes are listed below.
- **Model status** — if a specific model is failing, check whether it’s enabled in`GET /v1/models` .
- **Balance** — a`402 Payment Required` response means your balance is zero or a spend cap was hit. Check Dashboard →**Billing** .

## Common error codes

## The error catalog endpoint

`GET /v1/errors` returns the full error action-item catalog — the machine-readable
source behind the table above. Use it to surface a helpful message and next step
for any error your client receives:
`codes` with the specific error identifier, falling
back to `categories` by the error’s category, then to `default`.
The payload is small and slow-changing. Cache it, and re-fetch only when its

`version` field changes.
[Troubleshooting → Mesh API](/debug/mesh-api).

## Contacting support

**Email:**

[support@meshapi.ai](mailto:support@meshapi.ai)When writing in, include:

1. Your **request ID** (`req_...` from the response headers or Dashboard logs)
2. The **model** and**endpoint** you were calling
3. A **description of the issue** and any error message you received
4. The **account email** associated with your API key

## Billing issues

For payment failures, missing credits, or invoice questions:
[support@meshapi.ai](mailto:support@meshapi.ai)with subject line

**Billing**.

## Feature requests

Have an idea? Open a discussion or request at
[support@meshapi.ai](mailto:support@meshapi.ai)with subject line

**Feature Request**.

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/support
