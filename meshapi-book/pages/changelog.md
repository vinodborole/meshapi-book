---
type: Web Page
title: Changelog - Mesh API
description: New endpoints, breaking changes, deprecations, and SDK releases.
resource: https://developers.meshapi.ai/changelog
timestamp: '2026-09-14T12:21:17.301704+00:00'
---

Subscribe at 

[to hear about deprecations and breaking changes without checking back. Every entry that removes or changes existing behaviour is tagged](/changelog/rss.xml)`/changelog/rss.xml`**Deprecated**or**Changed**; where the change is a response shape, the entry says which API version carries it — see[API versioning](/docs/reference/api-versioning)for how to pin a version so a shape change cannot reach your code unannounced.
AddedChangedAPIVideo

API version 2026-09 — poll a video task by request id; unusable video inputs are refused

## Poll a video task with the id you already have

Version`2026-09` is out (released 3 September). Pin it and
`GET /v1/video/generations/{id}` accepts the `request_id` Mesh returned when you created
the task, as well as the provider task id:`404`,
never a leak.Video task bodies also gained a `request_id` field — on the create response, on
`GET /v1/video/generations/{id}`, and on every row of the `GET /v1/video/generations`
list. Under `2026-08` the field is stripped, so a pinned parser sees exactly what it saw
before.Two things worth knowing before you switch:
- Tasks created before 20 August 2026 predate the recorded request id and stay reachable by task id only.
- `2026-08` is still the baseline, so an unpinned request is unaffected. Send the header
(or[pin it on your key](/docs/reference/api-versioning#pinning-a-whole-api-key) ) to use
this.

`GET /v1/api-versions` now reports a `capabilities` list per version — `2026-09` carries
`video.poll_by_request_id` — so you can check what a version gives you without reading
the changelog. The list is cumulative: a later version includes everything an earlier one
introduced. See [API versioning](/docs/reference/api-versioning).

## A video input a model cannot take is now a `422`

Send a `content` item whose role the serving model does not accept — a closing frame to
a model with no concept of one, a reference video to a model that takes only images —
and the submit is now rejected with `422`. The message names the item by its position
in `content` and, where the item was reinterpreted on the way in, what it resolved to.
It used to be dropped: the generation ran without it and you were billed for a video
built from inputs you did not send.For example, `bytedance/seedance-1.0-lite` takes an opening frame but not a closing one,
so a second image sent as the `last_frame` is refused:
**Top-level parameters are unchanged.**A tunable the model does not implement is still accepted and reported in

`unsupported_params` — a dropped parameter still gives you the
video you asked for, tuned differently, while a dropped asset changes what was generated.
**This applies on every API version.**It is a refusal, not a response shape, so pinning

`X-Mesh-Version` does not hold the old behaviour.Check `supports_video_first_frame` and its five siblings on `GET /v1/models` before you
send, and `GET /v1/models/{model_id}/providers` to see whether any route takes the input.Full detail: [Finding out what a model accepts](/docs/capabilities/video-generation#finding-out-what-a-model-accepts).

## Pin the API contract to a date

Mesh now versions its public contract
**by date**, in a request header. The current version is

`2026-08` (released 15 August):`/v1` stays exactly where it is — it is a namespace, not a
version, and there is no `/v2` planned.
**You do not have to do anything.**Sending no header gets you the

*oldest*supported version, so publishing a new one never moves existing traffic.

- `GET /v1/api-versions` lists every version served, with its`status` , release date,
and which one is the`baseline` and which is`latest` .
- An API key can carry a pin, so code you do not control still gets a fixed contract —
set it on the key’s **API Version** field under**API Keys** . A header on the request
wins over the key’s pin.
- Every response echoes `X-Mesh-Version` with the version that served it.
- A version Mesh does not serve is rejected with `400 invalid_api_version` rather than
silently downgraded, so a typo’d pin fails immediately instead of quietly serving you
something else. The error body lists`supported_versions` .

`Deprecation` and
`Sunset` headers first, so monitoring can notice before anyone reads a page. Nothing is
deprecated today.Full detail: [API versioning](/docs/reference/api-versioning).

## Node.js SDK 2.0.0 — the request id rides on what each call returns

**Breaking:**

`MeshAPIConfig.onResponse` and the `ResponseInfo` type are gone. The hook
was configured once per client, so with several calls in flight it could not say which
response belonged to which call.The id now lives on the value the call returned:
- Every streaming method returns an `SSEStream<T>` , an`AsyncIterable` that also carries`requestId` as a promise. It stays readable after a mid-stream failure or an abort,
which is when you most want it.
- `SSEStream.cancel()` releases the connection for a stream you read the id from but
never iterated. It is also wired to`Symbol.asyncDispose` for`await using` .
- Breaking out of a `for await` over a stream now closes the connection; previously it
leaked the socket.
- Iterating one stream object twice no longer issues, and bills, a second request.
- Errors raised from a mid-stream error frame no longer report an empty `requestId` .

`onResponse` has to
change. See [Streaming](/sdk/streaming)and

[Error handling](/sdk/error-handling).

## SDK releases

- **Node.js 2.0.0** — request id on every returned value;`onResponse` removed (above).
- **Node.js 1.0.4** — repaired Responses API streaming and the realtime ESM entry point;
clearer error messages;`models.free()` /`models.paid()` now filter server-side via`?free=` ; the SDK version header matches the package version again.
- **Node.js 1.0.2** — exposed`x-request-id` on successful responses via`onResponse` (superseded by 2.0.0 four days later).

## Translate audio to English

`POST /v1/audio/translations` accepts audio in any language and returns the speech
translated to English. This is distinct from the transcribe-and-translate helper at
`POST /v1/audio/transcriptions/translate`, which transcribes first and then translates
the text.`model` is required — check `GET /v1/models` for models that support translation. A
model that does not is refused with `422`. See [Audio](/docs/capabilities/audio).

## Structured outputs in the SDKs

Python 0.1.12 and Node.js 1.0.0 add a`parse()` method on chat completions that sends a
JSON-schema `response_format` and returns a typed value instead of a string:
[Structured output](/sdk/structured-output).

## Realtime audio — GA protocol

Python 0.1.11 and Go v0.1.12 move the realtime client to the GA wire protocol: audio is sent as base64`input_audio_buffer.append` events, and output audio is decoded into the
message’s `audio` field rather than left as a base64 string. See
[Realtime](/sdk/realtime).

## SDK releases

- **Python 0.1.12** — structured outputs,`chat.completions.parse()` .
- **Python 0.1.11** — realtime GA protocol.
- **Python 0.1.10** — realtime TLS trusts the`certifi` bundle; stricter request params;
error mapping matches the gateway’s error codes;`image_bytes()` helper for reading
generated images; spec fields that had been dropped are now declared.
- **Python 0.1.9** — transcription`language_code` example fixed.
- **Python 0.1.8** — audio translations.
- **Go v0.1.12** — realtime GA protocol.
- **Go v0.1.11** — SSE buffer no longer truncates large frames; error mapping matches the
gateway’s error codes;`Bytes()` helper for generated images; missing spec fields added.
- **Go v0.1.10** — transcription`language_code` example fixed.
- **Go v0.1.9** — audio translations.
- **Node.js 1.0.0** — first stable release. Covers the full public data plane:
moderations, web search, router select, models search and get, Responses API list and
get, image edits, audio translations, plus structured outputs. The unreleased`/v1/documents` wrapper was removed ahead of 1.0.

[SDK overview](/sdk/overview)for install commands and the current version of each.

# Citations

1. Source page: https://developers.meshapi.ai/changelog
