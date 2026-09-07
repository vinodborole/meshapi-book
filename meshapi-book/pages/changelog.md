---
type: Web Page
title: Changelog - Mesh API
description: New endpoints, breaking changes, deprecations, and SDK releases.
resource: https://developers.meshapi.ai/changelog
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

Subscribe at 

[to hear about deprecations and breaking changes without checking back. Every entry that removes or changes an existing shape is tagged](/changelog/rss.xml)`/changelog/rss.xml`**Deprecated**or**Changed**— see[API versioning](/docs/reference/api-versioning)for what those mean and how to pin a version so a change cannot reach your code unannounced.
## Poll a video task with the id you already have

Version`2026-09` is out. Pin it and `GET /v1/video/generations/{id}` accepts the
`request_id` Mesh returned when you created the task, as well as the provider task id:`request_id`
field.Two things worth knowing before you switch:
- Tasks created before 20 August 2026 predate the recorded request id and stay reachable by task id only.
- `2026-08` is still the baseline, so an unpinned request is unaffected. Send the header
(or[pin it on your key](/docs/reference/api-versioning#pinning-a-whole-api-key) ) to use
this.

`GET /v1/api-versions` now reports a `capabilities` list per version, so you can check
what a version gives you without reading the changelog. See
[API versioning](/docs/reference/api-versioning).

## A video input a model cannot take is now a `422`

Send a `content` item whose role the serving model does not accept — a closing frame to
a model with no concept of one, a reference video to a model that takes only images —
and the submit is now rejected with `422` naming the item — by its position in
`content` on most models, by role alone on the rest.It used to be dropped. The generation ran without it and you were billed for a video
built from inputs you did not send.
**Top-level parameters are unchanged.**A tunable the model does not implement is still accepted and reported in

`unsupported_params` — a dropped parameter still gives you the
video you asked for, tuned differently, while a dropped asset changes what was generated.
**This applies on every API version.**It is a refusal, not a response shape, so pinning

`X-Mesh-Version` does not hold the old behaviour.Check `supports_video_first_frame` and its five siblings on `GET /v1/models` before you
send, and `GET /v1/models/{model_id}/providers` to see whether any route takes the input.Full detail: [Finding out what a model accepts](/docs/capabilities/video-generation#finding-out-what-a-model-accepts).

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
