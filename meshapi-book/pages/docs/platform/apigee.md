---
type: Web Page
title: Apigee - Mesh API
description: Route Mesh API traffic through your own Apigee proxy — the target settings
  you must change, what Apigee cannot carry, and how to verify the path end to end.
resource: https://developers.meshapi.ai/docs/platform/apigee
timestamp: '2026-08-17T07:05:01.394536+00:00'
---

**Google Apigee**, you can put a pass-through proxy in front of Mesh API and keep a single point where authentication, quotas, analytics and traffic policy are applied. Mesh API needs no change to support this, and neither does your client code — a correctly configured proxy is transparent, and every endpoint behaves identically through it and direct. “Correctly configured” is the whole job. Several Apigee target defaults break Mesh traffic — streaming responses get buffered, slow generation calls time out, and Mesh’s own error bodies get replaced by Apigee fault envelopes. This page covers the settings that matter, the traffic Apigee cannot carry at all, and how to prove the path works.

Mesh’s own docs use 

**gateway**for the Mesh router itself. On this page,**the proxy**always means your Apigee proxy, and**Mesh API**means the backend behind it.
## How the pieces fit

Client traffic terminates at Apigee, which forwards to Mesh API and returns the response unchanged. The proxy does not rewrite paths, alter request or response bodies, or inject its own authentication. Use a
**single catch-all route**. The path suffix after your base path is forwarded verbatim:

**new Mesh API endpoints work through your proxy with no proxy change**. Given how often the catalog gains capabilities, per-endpoint routing is a standing maintenance cost with no benefit.

## Base URL

Migrating an existing integration is a base-URL change only. Paths, methods, headers, request bodies and response shapes are all unchanged.`base_url` to your proxy and everything
else stays as documented in the [SDK reference](/sdk/overview).

## Required target settings

These are the settings that differ from Apigee’s defaults. Each one is required, and the last three all cause failures that look like Mesh problems but are not.
Start with a bare pass-through and no policies, so that any behaviour change is
attributable to the proxy itself. Add quotas, SpikeArrest, caching and
API-product keys one at a time afterwards.

## Authentication

The proxy passes the`Authorization` header straight through. Use the same
Mesh API key you would use directly:
Errors are relayed from Mesh unmodified — provided 

`success.codes` is set as
above.
If you want consumers to hold Apigee-issued credentials instead of a Mesh key,
that is an API product and developer app on top of this proxy, with the Mesh
key injected at the target. Set up the pass-through first and verify it, then
layer that on.
## Verify the path

1

Send a request through the proxy

**Response:**

2

Send the same request direct, and diff

Point the same call at 

`https://api.meshapi.ai` and compare. A response that
differs in anything but `id` and timing means the proxy is transforming
traffic it should be passing through.Keep this pair as your regression test. If behaviour changes later, running
both isolates the cause at once — a response that differs only through the
proxy points at your proxy configuration, and one that matches direct points
somewhere else entirely.
3

Check streaming separately

Re-send with 

`"stream": true` and confirm chunks arrive progressively rather
than all at once at the end. Buffering is the single most common
misconfiguration and a non-streaming test will not catch it.
## What Apigee cannot carry

### Realtime audio over WebSocket

**Apigee does not proxy WebSocket traffic.**This is a platform limitation, not a configuration issue — there is no proxy setting that enables it.

[Realtime transcription](/docs/capabilities/realtime-audio)clients must connect

**directly to Mesh API**, bypassing the proxy. Any network policy that assumes all traffic flows through Apigee needs an explicit exception for this endpoint.

### File uploads bypass the proxy

`POST /v1/files` returns a pre-signed Google Cloud Storage URL:
`PUT`s the bytes **directly to**. That transfer does not pass through Apigee, so proxy policies, quotas and analytics apply only to the metadata call that reserves the record — never to file content. Egress rules must allow the storage host. See

`storage.googleapis.com`
[Files & RAG](/docs/capabilities/rag)for the full upload flow.

### Payloads above ~10 MB

Apigee buffers messages with a default ceiling of roughly
**10 MB**. Typical Mesh payloads sit comfortably inside it:

Beyond that, requests may be rejected at the proxy with a 

`413` that does not
occur direct. Large file content should use the pre-signed upload flow above
rather than inline payloads.
### Slow calls against the default timeout

Generation endpoints routinely exceed Apigee’s default target timeout, which is why`io.timeout.millis` is on the required list. Observed durations through a
pass-through proxy:
[Video generation](/docs/capabilities/video-generation)is asynchronous — you poll a task, so no single call is long-running. A

`504` from the proxy on a call
that succeeds direct means this ceiling needs raising.
## Errors that are not proxy-related

Three Mesh API responses are easy to mistake for proxy faults during a cutover. Each reproduces identically against the backend direct, so diffing the two calls identifies them straight away.
Only the last is affected by the proxy at all. If you serve browser clients
through Apigee, allowlist the proxy-facing origin before you cut over.

## Verified coverage

Mesh verifies every documented endpoint through a pass-through Apigee proxy and against the backend direct, then diffs the two.
**No endpoint behaves differently through the proxy**— every group returns responses matching direct:

The realtime audio WebSocket is the sole exclusion, for the reason above.

## Troubleshooting

Anything that reproduces against 

`https://api.meshapi.ai` directly is not a
proxy issue — see [Troubleshooting](/debug/mesh-api)or contact

[support](/docs/reference/support).

# Citations

1. Source page: https://developers.meshapi.ai/docs/platform/apigee
