---
type: Web Page
title: Video Generation - Mesh API
description: Turn a prompt, a still image, a clip, or an audio track into video with
  one async API call.
resource: https://developers.meshapi.ai/docs/capabilities/video-generation
timestamp: '2026-09-07T12:05:08.101958+00:00'
---

`POST` returns a task ID straight away, and the finished video
arrives either when you [ask for it](#poll-for-the-result)or through a

[webhook](#have-the-result-delivered). Everything goes to

`https://api.meshapi.ai` and carries your API key as
`Authorization: Bearer rsk_<your-key>`. See
[Authentication](/docs/getting-started/authentication)if you need one.

## Your first video

A prompt in, a task ID out.
- curl
- Python
- Node.js

`id` — [Getting the result](#getting-the-result)turns it into a video. Log

[beside it while you are there: this is the only response that ever carries it, and it tells you whether the request that ran was the request you sent. Three endpoints cover everything on this page:](#what-the-route-dropped)

`unsupported_params`
The examples further down show the 

**request body**only. Send each of them exactly like the curl above — same URL, same two headers.
## Giving the model something to work from

The`content` array carries your prompt and every piece of media that goes with it.
Each item says two separate things:
- **`type`** is the*modality* —`text` ,`image_url` ,`video_url` or`audio_url` .
- **`role`** is what that media is*for* —`first_frame` ,`last_frame` ,`reference_image` ,`reference_video` or`reference_audio` .

`image_url` items are ambiguous on their own; a `first_frame` and a
`last_frame` are not.
Any of the URLs may be a publicly reachable link or a Base64 data URI
(

`data:image/jpeg;base64,...` — keep the MIME prefix). A public URL has to be
fetchable without authentication, since the model fetches it itself.
`role` is optional. Leave it out and each item falls back to its most common meaning:
So the simplest image-to-video request needs no 

`role` at all. You need one as soon as
a request pins a closing frame, or uses images as references rather than as frames.
A `role` that contradicts its item — `first_frame` on an audio item — is rejected
with `422`. So is a role the model you picked does not offer: rather than drop the
item and generate something you did not ask for, the submit refuses it and names the
offending entry — by its position in `content` on most models, by role alone on the
rest. Neither costs you a generation, but both cost you a round trip —
[check first](#finding-out-what-a-model-accepts).

### Pick one input scenario, not two

Roles are not freely combinable. A request is one of these shapes, and on most models they are
**mutually exclusive**:

To pin both ends 

*and*supply references, use first-and-last-frame — it is the only shape that guarantees the output matches the images you gave. References can be nudged toward a particular opening with wording in the prompt, but nothing guarantees the match. Text to video is not universal either: a model published specifically for image-to-video or for upscaling needs its input media and turns down a prompt-only request with a

`400`.
### Start from an image

One image and a prompt: the video opens on your still and moves from there.`role` is
optional here, but writing it makes the intent obvious.
### Open and close on exact frames

Give the model both ends and it fills in the middle. Here`role` is required — two
images with no roles are ambiguous.
The two images may be identical, and they need not share an aspect ratio — the first
frame wins and the last frame is cropped to match. A closing frame is a rarer ability
than an opening one, and models that offer it usually want a 

`first_frame` with it.
### Work from reference images

References say
*what should appear*, not

*where in time it appears*— a face, a product, a look. Use them when you want the same thing to show up in the video without dictating any particular frame.

`supports_video_reference_image` tells you whether the role is accepted at all;
the model governs how many.
### Name your subjects

A flat list of five reference images is five unrelated references — nothing in it says that the first two are the same person and the third is the bottle she picks up.`subject` says so. Pick a name, put it on every item that shows that subject, and
refer to it in the prompt as `@name`.
**Names are strict and match exactly.**1–64 characters of letters, digits, underscore or hyphen. Spaces,

`@`, dots and non-ASCII are rejected with a `422`, as is an empty
name, and matching is case-sensitive — `Hero` and `hero` are two different subjects.
In the prompt, only names you actually declared are treated as citations; every other
`@` in your text is left exactly as you typed it.
**On a**

`subject` belongs on reference items only.`first_frame`, on a `text` item, or
anywhere else it returns `422`.
**Grouping never costs you anything.**A model with no notion of subjects still receives every image as an ordinary reference, so the request above works wherever reference images do — it simply loses the distinction between the two subjects. You never need two versions of a request.

Not every model reads names. Some match reference images by the order you sent them
instead, and there an 

`@name` is just text in your prompt — the images still reach the
model, you only lose which-is-which. Describing your subjects in prose as well (“the
man in the blue coat picks up the green bottle”) costs nothing and reads correctly
either way.
### Keeping a character consistent

Models that can hold one character across a whole video generally screen the reference first, for likeness and for copyright, and decline anything that looks like a real person. That screening is the usual reason a reference which “should” work appears to have been ignored. Where a model works that way, Mesh registers your reference before generating so the same face survives from shot to shot. There is nothing to switch on — send the image as a public URL or a data URI exactly as you would otherwise.
**The first request with a brand-new reference can come back**Registration runs in the background with no fixed completion time. Retry the same request; the retry joins the registration already in flight rather than starting a second one, and after that the image is immediate.

`422`, saying the
reference is still being prepared.
**References are matched on content, not on URL.**The same image behind a re-signed link or a CDN variant is one reference, so changing the URL of an image you have already used costs nothing.

**Send the image, not an ID.**

`asset://` identifiers from elsewhere are rejected with
`422`. Supply a URL or a data URI and Mesh does the registering.
### Work from a clip

A`reference_video` is a clip for the model to imitate, edit or extend — motion, pace,
framing, or the footage itself.
`supports_video_reference_video` first. To raise the resolution of an existing video
rather than reimagine it, see [Upscale an existing video](#upscale-an-existing-video).

### Work from a soundtrack

A`reference_audio` item gives the model audio to follow — a beat to cut to, a voice to
match a mouth to.
`supports_video_reference_audio` is the flag. Some models also will not take audio as
the *only*media in a request; if one turns this down, send a reference image or a clip alongside it.

### Upscale an existing video

Upscalers take a video and return a higher-resolution one. The request is a different shape: the source clip is the
**only**input, and a prompt,

`ratio`, `duration` or
`frames` are not accepted.
`video_url` item is required — a
prompt on its own returns `400`, because there is nothing to upscale — and the target
resolution is fixed by the model, so `resolution` is informational here and does not
change the output.
### What you can send

The ceilings below are the widest any current model allows. A given model may be tighter, so treat a refusal that names a size, a duration or a count as that model’s limit rather than the platform’s.
**Images**—

`jpeg`, `png`, `webp`, `bmp`, `tiff`, `gif`, and `heic`/`heif` on some
models. Each side 300–6000 px, aspect ratio between 0.4 and 2.5, up to 30 MB each. One
image for a first frame, two for first-and-last, up to 30 references.
**Clips**—

`mp4` or `mov`, H.264/AVC or H.265/HEVC video with AAC or MP3 audio, 24–60
fps. Each side 300–6000 px, aspect ratio 0.4–2.5, up to 200 MB and 2–30 s each, up to
10 clips totalling 30 s. An edit job needs at least 4 s to work from.
**Audio**—

`wav` or `mp3`, up to 15 MB and 2–30 s each, up to 10 clips totalling 30 s.
### Older spellings

Before`role` existed, the purpose went in `type` — `"type": "first_frame"` and
friends. Those requests still work and still mean what they meant, so nothing you
already send needs changing. Write new ones with the modality in `type` and the purpose
in `role`: the older form is read as the role it always meant, so an item the model
cannot take is refused with a `422` either way. The modern spelling is the one that
works everywhere.
## Shaping the output

Everything on this page other than`model` and `content` is an optional top-level
field. Mesh sets no default for any of them — omitted means absent, not “the model’s
default”.
### The basics

### Finer control

These shape
*how*the model generates rather than what comes out of the pipe. Support varies by model, which is the whole reason for the

[capability flags](#finding-out-what-a-model-accepts).

`guidance_scale` and `num_inference_steps` are the only two values Mesh range-checks.
Every other field above is forwarded exactly as you wrote it, so the values listed are
the ones that mean something to the models implementing that field, not a set Mesh
polices. Send `output_format: "webm"` and you will not get a `422` from us — you get
whatever the model makes of an unfamiliar container, which may be an error and may be
nothing at all.
### Say which reference job you mean

Some models can do several different jobs from reference material: generate something new, edit an existing clip, or extend one. Each job has its own constraints, and if you let the model guess which one you meant, a request whose parameters contradict its guess is accepted and fails minutes later.`omni_reference_task_type` names the job so the constraints are checked at submit
instead:
Naming the job is worth it even when you are confident: the failure you avoid is the
expensive kind to debug.

The model still re-reads your prompt when it runs. If it decides you asked for a
different job than the one you named, the task fails anyway — so keep the prompt
consistent with the job.

### Preview first, then render

`draft: true` renders a fast, rough version of the same request. When you like one, send
its task ID back as a `draft_task` item and the model renders it properly instead of
generating something new from scratch.
## Finding out what a model accepts

Video models differ far more than text models do — one takes a closing frame, the next has no concept of one. Two fields answer the question: one before you send, one after.
### Check the flags before you send

`GET /v1/models` returns a capability flag per input, at the top level of each model
entry beside `supports_tools` and the rest. Read them at runtime rather than keeping a
list of your own: support genuinely changes between two builds of the same family.
`supports_video_generation`, which says the model makes video at all. These
six then describe what it takes **in**:

Five of the six are named for a content role. The last one is not — it is named for the
top-level 

`negative_prompt` parameter, and there is no `negative_prompt` role to put on
a `content` item.
A model can be served by more than one route, and the flags can differ between them.
`GET /v1/models` reports the route you are most likely to get;
`GET /v1/models/{model_id}/providers` returns the same six for every route that can
serve the model. So the model entry answers “will the route I am likely to get take a
last frame”, and the providers endpoint answers “does any route take one”.
[Models](/docs/reference/models#model-capabilities).

### What each model accepts

The flags say WHICH inputs a model takes. This says which VALUES its parameters accept — the resolutions it renders, how long a clip may be, the aspect ratios it offers, and how many media items you may attach. Read it before sizing a request: a value a model does not render is a`422` at
submit, and a parameter it does not implement is dropped and named in
[.](#what-the-route-dropped)

`unsupported_params`
<sub>135 models. A dash means the model does not take that parameter, or publishes no value for it.</sub>

### What the route dropped

A parameter the serving model does not implement is not an error. Mesh takes the request, generates the video without that parameter, and names it in`unsupported_params` on the response to your `POST`:
**It is**A truthiness check on

`null`, not `[]`, when nothing was dropped.`.length`
throws. Guard for null.
**It names top-level parameters only.**

`model` and `content` are excluded — but a
`content` item the model cannot take is never silently dropped either. It is refused
with a `422` at submit, because a dropped asset changes what gets generated and you
would be billed for it: a dropped *tunable*still gives you the video you asked for, tuned differently. Treat a non-empty

`unsupported_params` as something to fix rather than something to
live with: the request you meant to send and the request that ran are not the same one.
## Getting the result

Two ways to collect a finished video: ask for it, or have it delivered. You can do both — register a webhook for production and poll as a safety net. Usage logging is deduplicated, so you are never billed twice.
### Poll for the result

`GET /v1/video/generations/{id}` returns the task’s current state. While a task is
still running, Mesh fetches the latest status live before answering; once it has
finished, later calls are served straight from Mesh.
- Python
- curl

`status`, never on the
HTTP code: a task that failed still returns `200`.
A finished task looks like this:

Copy the video into your own storage when the task succeeds. Result URLs are there
for you to fetch the file, not to serve it from your product.

### Have the result delivered

Pass a`callback_url` on the create request and Mesh POSTs the finished task to it as
soon as the task reaches `succeeded`, `failed` or `expired`. No polling at all.
**omitted**rather than sent as

`null`. Read a missing key as null, not as an error.
### List your recent tasks

`GET /v1/video/generations` returns your tasks, newest first. It is served from Mesh
without checking upstream, so an in-progress task can show a stale status — call the
single-task endpoint for a live one.
## What a video costs

Video is billed per generation from the tokens on the finished task, and the rate depends on what you asked for:
Resolution matters too — 

`billed_resolution` on the finished task tells you which rate
you were charged at. Per-model rates are on the
[Pricing](/docs/getting-started/pricing)page, and

[Available Models](/docs/reference/models-list)carries the live numbers.

## When something is refused

### A task that failed after it was accepted

The most common surprise here: a video task can be accepted and fail later. The GET returns
**HTTP 200**with

`status: "failed"` and an `error` object — the failure is in
the body, not the status code.
`status`, and log `error.code` — `content_policy_violation` and
`invalid_input` are the two you will see most.
### Where to look first

### HTTP status reference

`429`, `502` and `503` are usually transient — retry with backoff. For anything else,
[Video generation troubleshooting](/debug/video-generation)walks through the specific symptoms, and the

[error reference](/docs/reference/errors)covers codes shared across the API.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/video-generation
