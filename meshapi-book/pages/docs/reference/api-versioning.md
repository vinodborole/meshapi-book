---
type: Web Page
title: API versioning - Mesh API
description: Mesh versions its contract by date. Pin a version with X-Mesh-Version
  so a future change to a response shape cannot change it underneath your code.
resource: https://developers.meshapi.ai/docs/reference/api-versioning
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

**by date**, not by a number in the URL. The newest version is

`2026-09`; if you send nothing you get `2026-08`. You select one with a header:
`/v1` in the path is **not**a version. It is a stable namespace and it is not going to change —

`/v2` is not planned. Everything about the shape of a request or response is
governed by the dated version instead.
## Why a version header

Additive changes — a new field, a new optional parameter, a new endpoint — are shipped continuously and do not need a version. You get them without doing anything. A dated version exists for the changes that are
*not*safe: renaming a field, removing one, or changing what a value means. Rather than break your integration or freeze the API forever, those changes land in a new dated version, and requests pinned to an older one keep receiving the older shape. Pinning is what makes that a guarantee rather than a coincidence. A pinned request states which shape your code can parse, so a change to the current shape cannot reach it.

## If you send no header

You are served the
**baseline**— the

*oldest*supported version, which today is

`2026-08`.
That is a deliberate choice, and worth understanding because it inverts what people
usually expect from a default:
Publishing a new version never moves existing traffic. Because the baseline is the
oldest supported version rather than the newest, an integration that sends no header
keeps getting the same shapes after a new version ships. You move only by pinning the
new version yourself.

## Which versions are served

`GET /v1/api-versions` lists them, oldest first. It takes your `rsk_` API key.
`baseline` marks what an unpinned request gets; `latest` marks the newest. They are now two
different entries — `latest` is deliberately **not**the default, so shipping

`2026-09` did
not move anyone.
`capabilities` lists behaviour a version *adds*, and it is cumulative: everything introduced at or before that label. Unlike a change to a response shape, a capability is something you can only use once you pin for it — see

[What each version added](#what-each-version-added).

## Pinning a whole API key

Rather than sending the header on every request, you can pin the key itself — useful when the calling code is not yours to change, or when a whole integration should sit on one version. Set it when creating a key, or on an existing one — key management takes an
[admin key](/docs/getting-started/api-keys)(

`mak_...`), not the `rsk_` key being pinned:
`"api_version": null` to clear it. In the dashboard the same setting is on the key’s
**API Version**field under

**API Keys**. Resolution order, highest first:

1. The `X-Mesh-Version`**header** on the request — always wins.
2. The **key’s** stored pin.
3. The **baseline** .

A key’s pin is resolved when the key is created, from your organisation’s default at that
moment — so a key created without an explicit version may show a concrete one afterwards
rather than staying empty. Changing your org default later does not move existing keys.

*your*header should be loud; a version we retired out from under a stored pin should not break your traffic.

## Unsupported versions are rejected, not downgraded

If you send a version Mesh does not serve, the request fails:`400`, with the versions that *are*served, and no fallback. Validation runs before authentication, so a bad version is rejected with

`400` even if
the key is also wrong.
## Reading the version that served a request

Every response carries`X-Mesh-Version` with the version that served it — including error
responses, and including requests that sent no header at all. So you can confirm what you
actually got rather than assume it, and you can see which version a stored key pin resolved to
without looking the key up.
## Format

A version label is`YYYY-MM` — four digits, a hyphen, two digits, e.g. `2026-08`. Anything
else is rejected with the same `400 invalid_api_version`.
Pass it as a literal string. Do not compute it from the current date: the label names a
contract, not today’s month, and there is no version for most months.
## Which version to pin

Pin the version you developed and tested against, and change it deliberately. Concretely: hold the label in one constant in your codebase, send it on every request, and treat bumping it as a change with its own review — because a new version exists precisely when some shape is different, so moving to it means reading what changed.
## What is not covered by a version

- **Model availability and pricing.** Models are added, retired, and repriced
continuously. Pin a version and you still get the current catalogue — see[Models](/docs/reference/models) .
- **Rate limits and quotas.** Governed by your plan, not the contract. See[Rate limits](/docs/getting-started/rate-limits) .
- **Provider behaviour.** A model’s output quality or latency is the provider’s, and no
version freezes it.
- **Bug fixes.** A response that did not match its documented shape gets corrected in
place, in every version.

## Realtime

The realtime WebSocket API (`/v1/realtime`) negotiates separately and does not read this
header. Sending it on the handshake has no effect.
## Knowing something is going away, without reading this page

When a version or an endpoint is scheduled for retirement, its responses start carrying standard headers — so your monitoring can find out before a human does:`Sunset` may be absent while `Deprecation` is present: “deprecated, retirement date to be
announced” is the normal state at announcement time. Absence means undecided, not imminent.
These arrive on **every**response for an affected route, including errors — a

`429` from a
deprecated endpoint is still deprecated, and an error response is often the only one a client
logs.
Nothing is deprecated today, so nothing currently emits them.
If you read these from browser JavaScript, note they are already in the CORS
`Access-Control-Expose-Headers` allowlist along with `X-Mesh-Version`.
## What each version added

Poll a video task by your request id

A video generation task can be fetched with the 

`request_id` Mesh returned for the request
that created it, as well as with the provider task id. Video task bodies also carry a
`request_id` field. Capability: `video.poll_by_request_id`. See
[Video generation](/docs/capabilities/video-generation#poll-for-the-result).
The baseline contract

The contract as it stood when dated versions were introduced. This is what you are served
if you send no header and have set no pin.

## Support window

Every version`GET /v1/api-versions` lists is served. Both `2026-08` and `2026-09` are `ga`,
nothing is scheduled for retirement, and no pin can go stale today.
A version is never withdrawn without notice. When one is scheduled for retirement it starts
returning `Deprecation` and `Sunset` response headers, so your client can detect it without
reading this page. If you need the retirement policy’s exact numbers before committing to a
pin, [contact support](/docs/reference/support).

# Citations

1. Source page: https://developers.meshapi.ai/docs/reference/api-versioning
