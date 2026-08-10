---
type: Web Page
title: Prompt Templates - Mesh API
description: Fix templates that don't render, don't resolve, or don't apply the model
  you expected.
resource: https://developers.meshapi.ai/debug/prompt-templates
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

`{{variable}}` values into a stored system prompt and
base messages. See [Prompt Templates](/docs/capabilities/prompt-templates)for the full guide.

## 422 — missing variables at inference time

Every`{{variable}}` slot the template renders must be supplied in the
`variables` object at inference time. A missing one returns a **. Extra variables you send are silently ignored.**

`422`
## Variables aren’t being substituted

- Syntax is double-brace: `{{ variable_name }}` (whitespace inside is trimmed, so`{{company}}` ==`{{ company }}` ).
- Names may contain letters, digits, and underscores only.
- The template’s `variables` list is**informational** — it documents expected slots but isn’t enforced server-side, so a typo in a slot name won’t be caught for you.

## Template not found / wrong template used

- **Name** lookups resolve to your key’s owner first, then fall back to global (admin-seeded) templates. If you own a template with the same name as a global one, yours wins.
- **UUID** lookups work across any owner — use the ID to share a template with another team.
- A name that matches nothing (and no global) fails to resolve.

## My per-request params aren’t taking effect

Parameters resolve highest-wins:
**request body > key defaults > template defaults**. If a value isn’t changing, something higher in that order is overriding it — e.g. a key default beating your template value.

## Still stuck?

See the
[Mesh API error reference](/debug/mesh-api#error-code-reference)or email

**.**

# Citations

1. Source page: https://developers.meshapi.ai/debug/prompt-templates
