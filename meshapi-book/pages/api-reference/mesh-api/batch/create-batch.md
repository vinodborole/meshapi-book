---
type: Web Page
title: Create Batch | Mesh API Docs
description: Create a batch job. Accepts requests inline — no separate file upload
  step required. Model and provider are resolved from body.model across all requests;
resource: https://developers.meshapi.ai/api-reference/mesh-api/batch/create-batch
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Create Batch

Create a batch job.
Accepts requests inline — no separate file upload step required.
Model and provider are resolved from `body.model` across all requests;
all requests must target the same model.
Returns 429 (batch_limit_exceeded) if the owner already has 10 or more
batches in a non-terminal state.
Returns 501 (not_implemented) if the resolved provider doesn't support batch.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

requests

completion_window

metadata

### Response

Batch job created

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/batch/create-batch
