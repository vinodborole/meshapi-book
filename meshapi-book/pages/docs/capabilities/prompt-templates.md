---
type: Web Page
title: Prompt Templates - Mesh API
description: Store, version, and reuse prompt configurations — with variable substitution
  — across your team and apps.
resource: https://developers.meshapi.ai/docs/capabilities/prompt-templates
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`{{variable}}` slots.
## Authentication

Template management (create, read, update, delete) accepts
**either**authentication method:

Using a template at inference time always uses your 

`rsk_...` key, the same as any other chat completion.
See the [Authentication guide](/docs/getting-started/authentication)for details on key types.

## Creating a Template

Send a`POST /v1/templates` request with a name, an optional system prompt, and any variables you want to parameterize.
- curl
- Node.js
- Python

**Response:**

## Using Variables

Variables use double-brace syntax:`{{ variable_name }}`. Whitespace inside the braces is trimmed, so `{{company}}` and `{{ company }}` are equivalent. Variable names may contain letters, digits, and underscores.
You can place variables in `system` and in any message `content` field:
`variables` field (list of strings) is informational — it documents which slots the template expects. It doesn’t enforce anything server-side, but it helps callers know what to pass.
**Missing variables at inference time cause a**Extra variables in the request are silently ignored.

`422` error.
## Using a Template in Chat Completions

Pass`template` (name or UUID) and `variables` alongside your `messages` in a `POST /v1/chat/completions` request. The template’s system prompt and base messages are prepended to your messages before the request is forwarded to the model.
- curl
- Node.js
- Python

You can reference a template by 

**name**or**UUID**. Name lookups are scoped to your key’s owner first, then fall back to global (admin-seeded) templates. UUID lookups work across any owner.
## Parameter Priority

When a template and a request body both specify inference settings, Mesh resolves them in this order (highest wins):
This means you can define sensible defaults in the template and still override them per-request without changing the template.

## Managing Templates

### List all templates

### Get a single template

### Update a template

Only the fields you include are changed — everything else stays the same.
### Delete a template

`204 No Content` on success.
## Global vs. Owned Templates

Global templates are useful for standard system roles (e.g., a generic coding assistant or translation prompt) that Mesh pre-seeds for all users. If you create a template with the same name as a global one, your owned template takes precedence in name lookups.

UUID-based lookups are intentionally cross-owner — this lets you share a
template with another team by passing them the template ID. Name-based lookups
are always scoped to your own account, so there’s no risk of accidentally
using someone else’s template by name.

# Citations

1. Source page: https://developers.meshapi.ai/docs/capabilities/prompt-templates
