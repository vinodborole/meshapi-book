---
type: Web Page
title: Service Tiers - Mesh API
description: 'Trade latency for a lower token price with service_tier: flex on OpenAI
  models — which models serve it, what it costs, and how the refusals behave.'
resource: https://developers.meshapi.ai/docs/capabilities/service-tiers
timestamp: '2026-09-21T12:28:28.555532+00:00'
---

`service_tier` is an optional field on `POST /v1/chat/completions` and
`POST /v1/responses`. It selects the capacity pool the provider serves your request
from.
Omitting the field is the same as sending 

`"default"`.
## Which models serve flex

Flex is an OpenAI feature. It is refused on every other provider, whatever model you name. Within OpenAI, only the models OpenAI publishes a complete flex rate card for can serve it. That set changes as OpenAI’s pricing moves, so read it from`GET /v1/models` rather than hardcoding a list:
- `pricing.flex_input_usd_per_1m` —**this is the signal to branch on.** A flex rate
is published only for a model whose provider can actually serve the tier, so its
presence is the closest thing to a yes.
- `supports_flex` — the model’s own declaration, and weaker than it looks. It does
not account for which upstream serves the model, so a row reached through a
provider with no flex tier still reports`true` .

Neither field is a guarantee, for two reasons. 

`GET /v1/models` publishes a model’s
**default**pricing row, while the tier is checked against the row for the provider your request actually resolves to — so a key with its own provider credential, a key pinned to a`fixed_provider`, or a brand routing rule can land on a row with no flex
tier. And a model can acquire a rate that flex does not price after its flex prices
were published. Treat a published flex rate as “expected to work” and handle the
`400` below.
## What it costs

Flex tokens bill at OpenAI’s published flex rates, materially below the standard rates. The rates are transcribed from OpenAI’s card per model rather than derived from a discount, so there is no ratio to apply — the per-model`pricing.flex_*`
values in `GET /v1/models` are the contract.
Prompt caching still applies on a flex request and bills at the model’s flex cache
rates. A flex request is **never**billed at a standard rate: if any rate the model carries has no flex counterpart, the request is refused rather than priced off the standard card. Flex is not the same product as the

[Batch API](/docs/capabilities/batch-api), which is asynchronous and priced off its own rate card.

## Sending a flex request

`POST /v1/responses`.
## A model that cannot serve flex is refused

A flex request for a model that cannot serve the tier is rejected with`400`
`model_capability_not_supported` before anything is sent upstream. It is **never**silently downgraded to the standard tier and billed at the standard price. Two messages, two causes. The provider your request resolved to has no flex tier, or the model publishes no flex input/output rate for the context length it would use:

`service_tier`, or pick a model that publishes a
flex rate.
## Knowing which tier actually served

A request can end up on the standard tier even though you asked for flex. If
[retry and fallback](/docs/platform/retry-and-fallback)moves the request to a different provider or model that cannot serve flex, the tier is dropped for that attempt and the request is served at the standard tier — and billed at that target’s standard rate, not at the flex rate you asked for. Two ways to see what happened:

## Caching is per tier

The
[gateway response cache](/docs/capabilities/caching)keys on the service tier, so a flex request never replays a response that was served at the standard tier, and vice versa. Two otherwise-identical requests on different tiers are two cache entries.

## Flex is slower by design

That is the trade you are making. Raise your client’s timeout before you use it — OpenAI recommends allowing up to 15 minutes for a flex request.
## When OpenAI’s flex capacity is full

Flex runs on spare capacity, so it can be unavailable when the standard tier is fine. OpenAI does not charge for a request refused this way, and neither do we:`503` with a `Retry-After` header. Mesh treats it as a capacity
condition rather than a rate limit, so the request is retryable and eligible for
fallback to another model. Two remedies:
- Retry with exponential backoff, honouring `Retry-After` .
- Retry without `service_tier` (or with`"auto"` ) to take standard capacity at the
standard price.

[Error Reference](/docs/reference/errors)for the full error contract.

## Where flex is not available

These are deliberate, not gaps:
The background refusal looks like this:

## Tracking flex spend

`service_tier` is recorded on every usage record and is filterable in usage
analytics, so flex spend can be separated from standard spend without tagging
requests yourself. See the [Usage API](/docs/reference/usage-api).

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/service-tiers
