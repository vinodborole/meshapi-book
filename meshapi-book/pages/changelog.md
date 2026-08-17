---
type: Web Page
title: Changelog - Mesh API
description: New endpoints, breaking changes, deprecations, and SDK releases.
resource: https://developers.meshapi.ai/changelog
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

Subscribe at 

[to hear about deprecations and breaking changes without checking back. Every entry that removes or changes an existing shape is tagged](/changelog/rss.xml)`/changelog/rss.xml`**Deprecated**or**Changed**— see[API versioning](/docs/reference/api-versioning)for what those mean and how to pin a version so a change cannot reach your code unannounced.
## Pin the API contract to a date

Mesh now versions its public contract
**by date**, in a request header. The current version is

`2026-08`:`/v1` stays exactly where it is — it is a namespace, not a
version, and there is no `/v2` planned.
**You do not have to do anything.**Sending no header gets you the

*oldest*supported version, so publishing a new one never moves existing traffic.

- `GET /v1/api-versions` lists every version served, and which one is the default.
- An API key can carry a pin, so code you do not control still gets a fixed contract —
set it on the key’s **API Version** field under**API Keys** .
- Every response echoes `X-Mesh-Version` with the version that served it.
- A version Mesh does not serve is rejected with `400 invalid_api_version` rather than
silently downgraded, so a typo’d pin fails immediately instead of quietly serving you
something else.

`Deprecation` and
`Sunset` headers first, so monitoring can notice before anyone reads a page. Nothing is
deprecated today.Full detail: [API versioning](/docs/reference/api-versioning).

## Translate audio to English

`POST /v1/audio/translations` accepts audio in any language and returns the speech
translated to English. This is distinct from the transcribe-and-translate helper at
`POST /v1/audio/transcriptions/translate`.`model` is required — check `GET /v1/models` for models that support translation.
## SDK releases

- **Python 0.1.8** — audio translations.
- **Go v0.1.9** — audio translations.

# Citations

1. Source page: https://developers.meshapi.ai/changelog
