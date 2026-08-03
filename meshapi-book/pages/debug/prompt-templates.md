---
type: Web Page
title: Prompt Templates | Mesh API Docs
description: Variable substitution, 422s, lookup scope, and parameter precedence.
resource: https://developers.meshapi.ai/debug/prompt-templates
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Prompt Templates

Variable substitution, 422s, lookup scope, and parameter precedence.

Prompt Templates inject `{{variable}}` values into a stored system prompt and
base messages. See [Prompt Templates](/templates) for the full guide.

## 422 — missing variables at inference time

Every `{{variable}}` slot the template renders must be supplied in the
`variables` object at inference time. A missing one returns a **`422`**. Extra
variables you send are silently ignored.

## Variables aren’t being substituted

- Syntax is double-brace: `{{ variable_name }}` (whitespace inside is trimmed, so`{{company}}` ==`{{ company }}` ).
- Names may contain letters, digits, and underscores only.
- The template’s `variables` list is**informational** — it documents expected slots but isn’t enforced server-side, so a typo in a slot name won’t be caught for you.

## Template not found / wrong template used

- **Name** lookups resolve to your key’s owner first, then fall back to global (admin-seeded) templates. If you own a template with the same name as a global one, yours wins.
- **UUID** lookups work across any owner — use the ID to share a template with another team.
- A name that matches nothing (and no global) fails to resolve.

## My per-request params aren’t taking effect

Parameters resolve highest-wins: **request body > key defaults > template
defaults**. If a value isn’t changing, something higher in that order is
overriding it — e.g. a key default beating your template value.

## Still stuck?

See the [Mesh API error reference](/debug/mesh-api#error-code-reference)
or email **[contact@meshapi.ai](mailto:contact@meshapi.ai)**.

# Citations

1. Source page: https://developers.meshapi.ai/debug/prompt-templates
