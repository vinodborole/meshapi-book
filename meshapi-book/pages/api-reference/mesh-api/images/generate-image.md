---
type: Web Page
title: Generate Image | Mesh API Docs
description: 'OpenAI-compatible image generation endpoint. > Note: The image response
  may not render in GitBook''s "Test it" panel.'
resource: https://developers.meshapi.ai/api-reference/mesh-api/images/generate-image
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# Generate Image

OpenAI-compatible image generation endpoint.

Note:The image response may not render in GitBook’s “Test it” panel. To view the generated image, try this endpoint with Postman, curl, or another HTTP client.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Request

This endpoint expects an object.

prompt

model

n

quality

response_format

size

aspect_ratio

resolution

output_format

output_compression

background

moderation

partial_images

image

seed

sequential_image_generation

sequential_image_generation_options

guidance_scale

watermark

optimize_prompt_options

stream

### Response

Successful Response

### Errors

401

Unauthorized Error

402

Payment Required Error

422

Unprocessable Entity Error

429

Too Many Requests Error

500

Internal Server Error

501

Not Implemented Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/images/generate-image
