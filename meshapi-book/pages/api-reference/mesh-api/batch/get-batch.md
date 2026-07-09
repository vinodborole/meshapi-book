---
type: Web Page
title: Get Batch | Mesh API Docs
description: Get batch status from the upstream provider. Usage logging and billing
  fire automatically on the first terminal observation.
resource: https://developers.meshapi.ai/api-reference/mesh-api/batch/get-batch
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Get Batch

Get batch status from the upstream provider.
Usage logging and billing fire automatically on the first terminal observation.
When the batch is completed and has an output file, the response includes a
`results` key with the parsed output (list of per-request response objects).

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Path parameters

batch_id

### Response

Batch details

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/batch/get-batch
