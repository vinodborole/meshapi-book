---
type: Web Page
title: Account Configuration Checklist - Mesh API
description: Configure funding, spend caps, keys, rate limits, fallbacks, caching,
  memory, templates, usage visibility, orgs, and BYOK — with the default for every
  setting.
resource: https://developers.meshapi.ai/docs/getting-started/account-checklist
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

- Work top to bottom — the order matters. Money first, then keys, then features.
- Every item shows the **default if not set** , so you know exactly what happens if you skip it.
- Most settings live in the [dashboard](/docs/getting-started/dashboard) . API fields are noted for developers.
- New to Mesh? Make your first call with the [Quickstart](/docs/getting-started/quickstart) , then come back here.

## 1. Add money and protect your spend

Mesh is prepaid: you add credits, and each request draws them down. See
[Pricing](/docs/getting-started/pricing)for how costs work.

- 
 **Add credits and check your balance.**  - Every paid request draws from your credit balance. At $0, requests are rejected with a `402` error.
  - You are billed after each call completes. So one large request can push your balance slightly below zero — that’s normal, and it settles on your next top-up.
  - **Default if not set:** no credits — paid requests fail with`402` .
  - **Where:** Dashboard → Billing. API:`GET /v1/balance` .
- Every paid request draws from your credit balance. At $0, requests are rejected with a 
- 
 **Turn on auto-recharge.**  - Tops up your balance automatically when it runs low, so a long job never dies mid-run with a `402` .
  - **Default if not set:** off — your balance can hit $0 and requests start failing.
  - **Recommended:** add $25 whenever the balance drops below ~$10. Each recharge can be $5–$100, and you can have one active auto-recharge setup per account.
  - **Where:** Dashboard → Billing → Auto-recharge. API:`POST /v1/auto-recharge/setup` , check status with`GET /v1/auto-recharge` .
- Tops up your balance automatically when it runs low, so a long job never dies mid-run with a 
- 
 **Set a spend cap on every key.**  - Caps how much a single key can spend.
  - **Default if not set:** no cap —**unlimited spend** on that key.
  - **Where:** Dashboard → API Keys. API:`spend_cap_usd` on`POST /v1/keys` or`PATCH /v1/keys/{id}` .
  - **Good to know:** caps are soft — a key can go over by at most one request before the`402` kicks in. If you use orgs, caps also exist per org, team, and member (over a rolling 30 days).

## 2. Create your API keys

Keys are how your apps sign in to Mesh. They are also the unit for limits, caps, and usage tracking. See
[Authentication](/docs/getting-started/authentication)and the

[Dashboard Guide](/docs/getting-started/dashboard).

- 
 **Create one key per app or environment.**  - Don’t share one key across apps. Separate keys let you see which app spent what — and cap each one.
  - The full key (`rsk_…` ) is shown**once** at creation. Store it somewhere safe.
  - **Default if not set:** you have no keys — you need at least one to make calls.
  - **Where:** Dashboard → API Keys. API:`POST /v1/keys` .
- 
 **Set a default model on each key.**  - Your apps can then leave out the `model` field.
  - If a request does name a model, the request wins. Order: request body → key default → template. Templates never override.
  - **Default if not set:** no default — requests that omit`model` fail.
  - **Where:** Dashboard → API Keys. API:`default_model` (and`default_params` ) on the key.
- Your apps can then leave out the 
- 
 **Leave rate limits alone unless you need more.**  - Limits how many requests a key can make, counted per minute and per day.
  - **Default if not set:** 100 requests/min and 10,000/day per key. Self-serve maximum is 1,000/min.
  - Going over returns a `429` error with a`Retry-After` header.
  - **Where:** Dashboard → API Keys. API: check effective limits with`GET /v1/keys/{id}/limits` (the lowest of key / member / org limits wins) or`GET /v1/usage/rate-limits` .
- 
 **Skip model allow-lists unless you truly need one.**  - The `allowed_models` field restricts a key to a fixed list of models. It’s easy to get wrong:
    - Web search runs as `perplexity/sonar` . Leave that off the list and`POST /v1/web/search` returns`403` .
    - `model="auto"` fails whenever the router picks a model that isn’t on your list.
  - Web search runs as 
  - **Default if not set:** empty — all models allowed. Recommended for most accounts.
  - **Where:** API:`allowed_models` on the key. If you do use it, include**every** model your features touch.
- The 
- 
 **Optional: set per-model budgets.**  - Cap the cost, tokens, or request count for a specific model on a key.
  - **Default if not set:** no per-model budgets.
  - **Where:** API:`model_limits` (`max_cost_usd` /`max_tokens` /`max_requests` ) on the key.

## 3. Keep requests reliable

Mesh can pick models for you and route around outages. See
[Auto Routing](/docs/capabilities/auto-routing).

- 
 **Let Mesh pick the model.**  - Set the model to `"auto"` and Mesh picks a good, cost-effective model for each request — and avoids models that are down.
  - **Default if not set:** requests go to exactly the model you name, with no automatic rerouting.
  - **Where:**`model: "auto"` in the request body.
- Set the model to 
- 
 **Or pick from your own shortlist.**  - Send a list of models you’re happy with, and Mesh returns the best one that’s available right now.
  - **Default if not set:** n/a — this is opt-in per request.
  - **Where:** API:`POST /v1/router/select` with`candidate_models` .
- 
 **BYOK users: leave fallback on.**  - If your own provider key fails (auth or quota errors), Mesh retries on its own credentials so your traffic keeps flowing.
  - **Default if not set:** on (`allow_fallback: true` ). Turn it off only if compliance demands strict BYOK-only — requests then fail hard.
  - **Where:**`allow_fallback` on your provider key. See[BYOK](/docs/capabilities/byok) .
- 
 **Retry and failover policies (early access).**  - Per-key automatic retries, plus failover to the same model on another provider.
  - **Default if not set:** off — currently enabled selectively, account by account.
  - **Where:**`routing_policy` on a key. Talk to your Mesh contact before relying on it.

## 4. Cut costs with caching

- 
 **Use the built-in response cache — repeat answers are free.**  - Send the same request twice within 24 hours and the second response costs **$0** . You’ll see the header`X-Cache: HIT` .
  - To qualify: `temperature: 0` (or leave it out) and no`tools` in the request.
  - The cache is private to your account — your entries are never shared. Entries last 24 hours.
  - **Default if not set:** on — you get this automatically whenever a request qualifies.
  - **Opt out per request:**`"cache": false` in the body, or the`X-Mesh-Cache: no-store` header.
- Send the same request twice within 24 hours and the second response costs 
- 
 **Cache big, stable prompts at the provider.**  - On supported models (for example Anthropic), mark long system prompts with `cache_control` . Cached input then bills at a discounted cache-read rate.
  - **Default if not set:** no provider caching — you pay the full input price every time.
  - **Where:**`cache_control` markers in the request. Discounted cache rates are listed per model on`GET /v1/models` .
- On supported models (for example Anthropic), mark long system prompts with 
- 
 **Batch anything that isn’t urgent.**  - Batch jobs run at cheaper batch pricing where the model supports it. See [Batch API](/docs/capabilities/batch-api) .
  - **Default if not set:** everything runs (and bills) at normal real-time pricing.
  - **Where:** API:`POST /v1/batches` .
- Batch jobs run at cheaper batch pricing where the model supports it. See 

## 5. Memory (optional)

Skip this section if you don’t use Mesh Memory.
- 
 **Create memories, then attach them to requests.**  - Attach with the `x-mem-id` header: comma-separated names or IDs, up to 16 per request. If entries conflict, the one listed first wins.
  - **Default if not set:** no memory — requests only know what’s in them.
  - **Where:** API:`/v1/memories` and`/v1/memories/{slug}/items` .
  - **Billing:** injected memory text bills like normal prompt tokens (capped at roughly 1,500 tokens per request). Each lookup also adds a small embedding charge (`memory_search_embed` ).
- Attach with the 
- 
 **Use the right type for each memory item.**  - `guardrail` — hard rules, always included word-for-word.
  - `preference` — stable style or configuration choices.
  - `fact` — long-tail knowledge; the 8 most relevant facts are pulled in per request.
  - **Default if not set:** n/a — you choose a type when adding each item.

## 6. Prompt templates

-  **Use templates for prompts your team shares.**  - A template stores messages and `{{variables}}` once, so every app uses the same prompt. Pick one per request with the`template` field (name or ID).
  - **Default if not set:** no template — each request carries its own full prompt.
  - **Where:** Dashboard → Templates. API:`template` field on the request.
  - **Good to know:** a template’s model and params are only**suggestions** . Both the request and the key defaults override them — don’t use a template to enforce a model.
  - **Learn more:**[Prompt Templates](/docs/capabilities/prompt-templates) .
- A template stores messages and 

## 7. Watch your usage

See
[Pricing](/docs/getting-started/pricing)for how costs are calculated.

- 
 **Check your spend regularly.**  - **Default if not set:** usage tracking is always on — there’s nothing to enable. This step is about knowing where to look.
  - **Where:** Dashboard → Billing (balance and spend history) and Dashboard → Logs (per-request detail). API:`GET /v1/balance` (live balance),`POST /v1/usage` (aggregates),`GET /v1/usage/spend-trend` ,`GET /v1/usage/events/export` (CSV for finance), plus org-level`GET /v1/orgs/current/usage` and`GET /v1/orgs/current/spend` .
- 
 **Know how a request is priced.**  - Cost = input rate × prompt tokens + output rate × completion tokens, plus per-call fees where shown.
  - Every model’s rates are public on `GET /v1/models` (and Dashboard → Models) — including cache read/write, long-context, and image/audio rates, and any flat per-request fee.
  - Web search bills a flat **$0.005 per successful search** . Failed or empty searches are free.
  - **Default if not set:** n/a — these rates apply to every account.

## 8. Teams & orgs (for companies)

- 
 **Create an org.**  - A default team is created for you automatically.
  - **Default if not set:** no org — your account stands alone, which is fine for individuals and small projects.
  - **Where:** API:`POST /v1/orgs` .
- 
 **Invite your teammates.**  - Roles: `admin` or`member` .
  - **Default if not set:** it’s just you.
  - **Where:** API:`POST /v1/orgs/current/members` (creates the invitation). List pending ones with`GET /v1/orgs/current/invitations` .
- Roles: 
- 
 **Give each team its own keys.**  - Create keys tied to a team so usage and limits count against the right team.
  - **Default if not set:** keys stay personal — usage isn’t attributed to any team.
  - **Where:** API:`POST /v1/keys` with`org_id` and`team_id` .
- 
 **Set limits per org, team, or member if you need internal cost control.**  - Requests/min, requests/day, tokens/min, and 30-day spend caps at each level. The lowest applicable limit wins.
  - **Default if not set:****all unlimited.**
  - **Where:** API: limit settings on the org, team, or member.

## 9. Bring your own provider keys (BYOK)

Full guide:
[BYOK](/docs/capabilities/byok).

- 
 **Decide if you need BYOK at all.**  - Use it when you want your own provider quota or negotiated rates, data-residency control, or provider-side features tied to your own account.
  - **Default if not set:** requests run on Mesh’s provider credentials — the right choice for most accounts.
  - **Pricing:** BYOK traffic carries a small platform fee — 5% of upstream cost, with the first 1M tokens each month fee-free.
- 
 **Add and test your provider keys.**  - Supported providers: AWS, Azure, Azure Foundry, OpenAI, Vertex. Each has a `/test` endpoint — validate before you rely on a key.
  - Your secrets are stored in a dedicated secret manager, never in the database.
  - **Default if not set:** n/a — BYOK is opt-in.
  - **Where:** API:`POST /v1/provider-keys/{aws|azure|azure-foundry|openai|vertex}` .
- Supported providers: AWS, Azure, Azure Foundry, OpenAI, Vertex. Each has a 
- 
 **Leave fallback on** (`allow_fallback: true` ) — see section 3.
  - **Default if not set:** on.

## Defaults if you don’t opt in

## Common surprises

## Need help?

- **Include the request ID.** Every API response carries a request ID (`req_…` ). Include it when you report an issue — it makes debugging fast.
- **Check the dashboard first.** For spend or limit questions, the usage and balance pages usually have the answer.
- **Talk to your Mesh contact.** For anything beyond self-serve — rate limits above 1,000 requests/min, retry policies, or billing questions — reach out to your Mesh contact or account manager.

# Citations

1. Source page: https://developers.meshapi.ai/docs/getting-started/account-checklist
