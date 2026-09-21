---
type: Web Page
title: Routing Algorithms - Mesh API
description: How the Auto Router picks a model — the four algorithms, the scoring
  formula, and the weights you can tune.
resource: https://developers.meshapi.ai/docs/capabilities/routing-algorithms
timestamp: '2026-09-21T12:28:28.555532+00:00'
---

[Auto Routing](/docs/capabilities/auto-routing)covers how to

*use*

`model: "auto"`.
This page covers how the pick is actually made, and the one knob you can turn.
## Two layers

A routing decision goes through two layers, and the word “weight” means something different in each:
1. **Which algorithm chain runs** — traffic is split across named*paths* by**traffic weights** . Operator-configured; you observe the result, you don’t set it.
2. **Which model that algorithm picks** — the`weighted` algorithm scores every
candidate with**scoring weights** over quality, cost, and latency. You*can* influence this per request, with[`weight_profile`](#choosing-a-profile) .

## The algorithms

Each path is an
**ordered waterfall**. Algorithms run in sequence and the first one that returns a model wins; an algorithm that declines (“abstains”) costs nothing and falls through to the next. Every algorithm has its own timeout, and none of them can fail your request — see

[Nothing here can fail a request](#nothing-here-can-fail-a-request).

`heuristic` is deliberately conservative. Any hint of task work, recency, code, links,
digits, or length over ~140 characters makes it decline, so the request takes the full
path instead. Over-declining is harmless; a wrong fast-lane pick is not.
### What the pool is

Every algorithm picks from the same
**candidate pool**: the models the gateway currently serves for that surface, already filtered by your key’s model policy and the request’s capability requirements. An algorithm can only ever return a model you were entitled to call directly.

## Traffic weights

The active configuration names one or more paths, each with a relative weight, and a path is chosen per request by weighted random.
**relative positive integers**, normalised internally —

`[90, 10]` and
`[9, 1]` are identical. The shape above is illustrative; the live split is an
operating decision and changes without an API change.
## Scoring weights

When the`weighted` algorithm runs, it scores every candidate in the pool:
### The three signals

All three are normalised so
**higher is always better**, which is why cost and latency are inverted — a cheap model scores high on

`C`, a fast one scores high on `L`.
Only the 

*order*of a measured index matters, never its magnitude, and a candidate with no measured score keeps its curated position rather than dropping. Nothing falls out of the pool for want of a benchmark.
### Missing signals degrade, they don’t error

A candidate missing a signal drops that term, and its remaining weights are renormalised to sum to 1. A candidate with neither cost nor latency data is scored on quality alone — so when the whole pool has no cost or latency data,`weighted` returns
exactly what `benchmark` would have.
## Weight profiles

The`(w_q, w_c, w_l)` vector is named. Four profiles ship:
Because each candidate’s present weights are renormalised, only the 

**ratio**between

`w_q`, `w_c` and `w_l` affects the outcome — not the absolute sum.
## Choosing a profile

Send`weight_profile` on a chat completions request:
- curl
- Header
- Python SDK

### Precedence

The effective profile is resolved highest-first:
1. **Request** — the`weight_profile` body field, or the`X-Mesh-Weight-Profile` header
2. **API key** — the`weight_profile` entry in the key’s routing policy
3. **Team / organization** — the same entry on the key’s routing-policy template, else your org’s default template
4. **Gateway default** —`balanced`

`weight_profile` applies to **, and only while**

`POST /v1/chat/completions` only`weighted` is the algorithm serving the request. On `/v1/responses` and
`/v1/router/select` it is ignored and the gateway default applies. Sending it is
always safe — it is stripped before your request reaches the upstream provider.
## Previewing a decision

`POST /v1/router/select` returns the model the Auto Router *would*pick, without running inference or billing you for one. Useful for pinning a model yourself, or for checking what a prompt classifies as.

`candidate_models` restricts it to ids you name
(intersected with your key’s policy — an empty intersection is a 422), and
`exclude_models` removes ids from consideration, which is how an “ask another model”
flow avoids re-picking the one already shown.
The classifier still runs, so a select call takes roughly as long as the routing stage
of a real request. 

`reasoning_effort` is a hint and is `null` unless the `benchmark`
algorithm classified the request.
## Nothing here can fail a request

`model: "auto"` always resolves to a concrete model. Every layer is fail-soft by
design:
- An algorithm that declines, times out, or errors falls through to the next in the chain.
- A chain that exhausts every algorithm falls through to the configured default model.
- A missing scoring signal drops that term instead of dropping the candidate.
- An unknown `weight_profile` — at any precedence tier — degrades to`balanced` .
- A settings-store outage falls back to the built-in defaults rather than refusing to route.

`x_auto_routed_fallback`
and `x_auto_routed_fallback_reason` on the body, or `X-Auto-Routed-Fallback` and
`X-Auto-Routed-Fallback-Reason` on a stream. See
[Response metadata](/docs/capabilities/auto-routing#response-metadata)for the full shape, and

[Debug → Auto Routing](/debug/auto-routing)when a pick surprises you.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/routing-algorithms
