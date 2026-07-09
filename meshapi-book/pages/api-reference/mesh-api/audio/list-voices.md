---
type: Web Page
title: List Voices | Mesh API Docs
description: 'List available TTS voices across every provider, filterable by model
  brand. A unified, normalised catalog spanning ElevenLabs, Together, and DeepInfra:'
resource: https://developers.meshapi.ai/api-reference/mesh-api/audio/list-voices
timestamp: '2026-07-09T11:31:58.280663+00:00'
---

# List Voices

List available TTS voices across every provider, filterable by model `brand`.
A unified, normalised catalog spanning ElevenLabs, Together, and DeepInfra:
  - **Together** voices are fetched live from its `/v1/voices` endpoint
    (Cartesia and any other dynamic models).
  - **Kokoro / Orpheus** preset voices (shared by Together + DeepInfra) come
    from a static catalog — DeepInfra exposes no per-model preset listing.
  - **ElevenLabs** account voices are fetched live.
Filters (case-insensitive): `brand` (e.g. `elevenlabs`, `hexgrad`,
`canopylabs`, `cartesia`), `model` (canonical model_id), and `search`
(substring match on voice id / name). Per-provider fetches are best-effort —
a provider that errors is skipped rather than failing the whole listing.

### Authentication

AuthorizationBearer

Bearer authentication of the form `Bearer <token>`, where token is your auth token.

### Query parameters

brand

model

search

### Response

Successful Response

voices

count

truncated

### Errors

422

Unprocessable Entity Error

# Citations

1. Source page: https://developers.meshapi.ai/api-reference/mesh-api/audio/list-voices
