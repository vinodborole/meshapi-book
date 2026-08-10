---
type: Web Page
title: Memory - Mesh API
description: Store per-user guardrails, preferences, and facts once, then attach them
  to any request with a single header.
resource: https://developers.meshapi.ai/docs/capabilities/memory
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

**memID**, then attach that memID to a chat completion with one header. Mesh composes the stored items into a leading system message before the request goes upstream. This is not conversation history. It is the small, stable set of things that should be true of

*every*request for that user.

## Authentication

Memory management accepts
**either**authentication method:

Both resolve to the same owner, so the memories your backend writes are the ones the dashboard shows.
Attaching a memory at inference time always uses your 

`rsk_...` key, the same as any other chat completion.
## Creating a memID

`slug` is the memID — the value you will send in the `x-mem-id` header. Pick something stable and derived from your own user id: **it cannot be renamed**, because requests already sending it would stop resolving. A slug may not be UUID-shaped (that would be ambiguous with a memory id) and may not contain spaces.

`description` is for your own reference and is never sent to the model.
## Adding items

An item is a
**guardrail**, a

**preference**, or a

**fact**. The three behave differently at request time, and choosing correctly matters more than anything else on this page.

Guardrails are the only type with a completeness guarantee. Anything that must never be silently dropped — a compliance rule, a hard constraint — belongs in a 

`guardrail`, not a `preference`.
A keyed preference is how you avoid contradictions when a request attaches more than one memID:
Facts are embedded when you add or edit them, which is a billed embedding
call. Guardrails and preferences are not embedded — they are always
deterministic.

## Attaching memory to a request

Send the memID in the`x-mem-id` header:
**survived**the token budget — so it is the answer to “did my memory actually reach the model”, not just “did I ask for it”.

### Multiple memIDs

Attach up to
**16**, comma-separated. Order is precedence, left-most wins:

- All distinct **guardrails** from every memID ship, deduplicated by text.
- **Preferences** sharing a`key` collapse to one — the left-most memID’s value.
- **Facts** from all attached memIDs compete for relevance within one budget.

`x-mem-id` takes **slugs**, not memory ids. The management API accepts either a slug or a UUID, but the header always matches on slug.

### How facts are selected

Guardrails and preferences are deterministic — the same items resolve on every request. Facts are not: Mesh embeds your latest user message and retrieves the facts closest to it, so a memID holding hundreds of facts contributes only the handful that bear on the question in front of it.
That retrieval issues one embedding call, billed to your account and recorded
in your usage as 

`memory_search_embed`. It is small, but it is not free — and
it is one call per request, not one per attached memID.
### Token budget

The assembled block is capped at roughly
**1,500 tokens**. Guardrails are kept in full and are never counted against that cap; preferences, then facts, are added while the block stays inside it, and a line that would overflow is dropped. A memID whose every line was dropped is

*not*listed in

`X-Mesh-Memory-Applied`.
Injected text becomes part of the real prompt sent to the provider, so **it is tokenized and billed like any other prompt content**. A large memID raises the cost of every request that attaches it.

## Retention

By default items are kept until you delete them. Set`retention_days` and Mesh stops sending — then deletes — preferences and facts that age past it:
- **Guardrails are exempt.** They never expire. A retention policy is for stored personal data, and silently dropping a safety rule is the opposite of what you asked for.
- **Existing items are re-dated from when they were created** , not from when you set the policy. Setting 30 days on a memory holding a 40-day-old fact expires that fact immediately.
- **Expiry takes effect on read.** An expired item stops being sent straight away; the row is deleted by a nightly sweep shortly after.

`"retention_days": null` to go back to keeping items forever.
## Reviewing and editing

### Inspect a memID

`expires_at` on each and `item_counts` by type.
### List memIDs

`q` matches on slug and name. Organisation owners and admins see every memID in the organisation; members see their own.
### Edit an item

Only the fields you include change:`item_type` cannot be changed — it decides both how the item is composed and whether it is indexed for search. Delete and re-add instead. Editing a fact re-indexes it.
### Turn a memID off without deleting it

### Delete

`204 No Content`. Deleting a memID removes its items and its stored facts from search, and is not reversible — it is the primitive to reach for when an end user asks you to erase what you hold about them.
## Usage

`injected_tokens_est` is Mesh’s own count of the block it prepended — no
provider reports the split between injected memory and the rest of your
prompt. Treat it as an estimate, not a charge. `tracked_since` tells you how
far back attribution data actually goes.
## Interactions worth knowing

**Memory failures are silent by design.**If a memID does not exist, is inactive, belongs to someone else, or the lookup itself fails, the request proceeds

*without*the memory rather than erroring. A memory problem can never take down your inference path — but a typo in

`x-mem-id` produces a perfectly successful, completely un-personalized response. `X-Mesh-Memory-Applied` is how you tell those two apart.
**Spend caps are re-checked after injection.**Injected tokens grow the prompt, so the cap is evaluated again against the assembled request: a key close to its limit can be rejected once memory is attached, even though the raw request would have passed.

## Who can see what

Two asymmetries are deliberate. Admins can 

**delete**a colleague’s memID but not edit it, because deletion is the right-to-be-forgotten primitive and has to work after someone has left the company. And attachment stays with the owner: the gateway matches

`x-mem-id` against the calling key’s own owner, so an admin can read a memID in the dashboard that their own keys cannot use. The dashboard labels those rows rather than offering a header snippet that would quietly do nothing.
## Related

- [Caching](/docs/capabilities/caching) — why memory-attached requests bypass the response cache
- [Prompt Templates](/docs/capabilities/prompt-templates) — static, shared prompt scaffolding, versus memory’s per-user context
- [Usage & Monitoring API](/docs/reference/usage-api) — where`memory_search_embed` charges show up

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/memory
