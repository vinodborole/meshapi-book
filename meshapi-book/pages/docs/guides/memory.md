---
type: Web Page
title: Memory | Mesh API Docs
description: Give models durable, per-user context — preferences, guardrails, and
  facts — that is injected automatically at inference time.
resource: https://developers.meshapi.ai/docs/guides/memory
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Memory

Memory lets you store durable context about a user and attach it to any chat completion with a single header. Instead of rebuilding a system prompt on every request, you write a preference or a fact once and reference the memory by name.

A **memory** is a named bucket (its *memID*, e.g. `mem_user_prefs`) holding **memory items**. When you attach a memory to a request, MeshAPI assembles the relevant items into a system message and prepends it to your conversation.

## Item types

Every item has one of three types, and the type determines how it is treated at injection time:

The distinction matters: guardrails are the only type with a completeness guarantee. Anything that must never be silently dropped — a compliance rule, a hard constraint — belongs in a `guardrail`, not a `preference`.

## Authentication

The two halves of this feature use **different credentials**, which is the most common source of confusion:

- **Managing memories** (`/v1/memories/**` ) requires a**user JWT** — the token your dashboard session uses. An`rsk_...` API key will not work.
- **Using a memory at inference** (the`x-mem-id` header) uses your normal**`rsk_...` API key** , like any other chat completion.

Memories are strictly owner-scoped in both directions: you can only read or modify your own, and a request can only ever attach memories owned by the caller.

## Creating a memory

###### curl

The `slug` is the memID you will pass in `x-mem-id`. Choose something stable — it is the handle you use everywhere else.

### Adding items

### Managing memories

Setting a memory inactive via `PATCH` stops it being injected without deleting its contents.

## Using memory at inference

Attach one or more memories with the `x-mem-id` header. Values are comma-separated, and **order is priority** — leftmost wins when two memories conflict.

MeshAPI resolves the memories, builds a single system-context block, and inserts it as the first message. The rest of your request is unchanged.

### How facts are selected

Guardrails and preferences are deterministic — the same items resolve every time. Facts are different: MeshAPI embeds your latest user message and retrieves the most relevant facts by semantic similarity, so a memory holding hundreds of facts contributes only the handful that matter to the current question.

Fact retrieval issues an embedding call, which is **billed to your account** and appears in your usage records as `memory_search_embed`. It is small, but it is not free — a request attaching a fact-bearing memory costs slightly more than the same request without one.

### Token budget

The assembled block is capped at roughly **1,500 tokens**. When the items exceed it, guardrails are kept in full and preferences and facts are dropped to fit.

Injected text becomes part of the real prompt sent to the provider, so **it is tokenized and billed like any other prompt content**. A large memory raises the cost of every request that attaches it.

## Interactions worth knowing

**Memory-attached requests are never cached.** A request carrying `x-mem-id` is personalized to one user, so serving it from the shared [response cache](/docs/guides/caching) could leak one user’s context to another. MeshAPI unconditionally bypasses both the cache read and the cache write for these requests.

If you rely on the gateway cache for cost control, be aware that adding memory to a request removes that saving entirely — you pay full price for every call.

**Memory failures are silent by design.** If a memID does not exist, is inactive, belongs to someone else, or the lookup fails, the request proceeds normally *without* the memory rather than erroring. This keeps a memory problem from taking down your inference path, but it also means a typo in `x-mem-id` produces a perfectly successful, completely un-personalized response. Verify your memIDs when a request behaves as if memory were not applied.

**Spend caps are re-checked after injection.** Because injected tokens grow the prompt, the spend cap is evaluated again against the assembled request — a key close to its cap can be rejected once memory is attached even though the raw request would have passed.

## Related

- [Caching](/docs/guides/caching) — why memory-attached requests bypass the response cache
- [Prompt Templates](/docs/guides/prompt-templates) — static, shared prompt scaffolding, versus memory’s per-user context
- [Usage & Monitoring API](/docs/guides/usage-monitoring-api) — finding`memory_search_embed` charges

# Citations

1. Source page: https://developers.meshapi.ai/docs/guides/memory
