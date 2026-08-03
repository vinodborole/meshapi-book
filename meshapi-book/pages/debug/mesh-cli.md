---
type: Web Page
title: Mesh CLI | Mesh API Docs
description: Fix the most common issues installing and running the Mesh CLI.
resource: https://developers.meshapi.ai/debug/mesh-cli
timestamp: '2026-08-03T09:56:31.586687+00:00'
---

# Mesh CLI

New to the CLI? Start with [Install the Mesh CLI](/cli/install) and
[Using the Mesh CLI](/cli/usage). If something isn’t working, find your symptom
below.

## ‘meshapi: command not found’ / ‘not recognized’

The install worked, but your terminal hasn’t picked up the new PATH — almost
always because the **close-and-reopen** step was skipped.

1. Close the Terminal / PowerShell window completely and open a new one, then try `meshapi` again.
2. Still failing? Re-run `pipx ensurepath` , then open**another** new window:

## ‘py’ or ‘python3: command not found’

Python isn’t installed, or (on Windows) the PATH checkbox wasn’t ticked during install.

- Redo [Install → Step 1](/cli/install) .
- On Windows, make sure you tick **“Add python.exe to PATH”** on the first installer screen before clicking Install.

## pipx mentions a missing ‘uv’ backend

Install with the pip backend instead:

## Wrong key or typo

Re-enter your API key from inside the CLI — no need to restart:

Then paste the correct `rsk_...` key. Grab a fresh one from
[app.meshapi.ai](https://app.meshapi.ai/) → **API Keys** if needed.

## ‘Insufficient balance’ (402)

Your account balance is exhausted.

- Top up at [app.meshapi.ai](https://app.meshapi.ai/) ,**or**
- Switch to a cheaper model with `/model` (use`/models` to compare prices).

This is the same `spend_limit_exceeded` / `402` you’d see from the API — see
[Mesh API debugging](/debug/mesh-api#error-code-reference).

## Updating to a newer version

The CLI offers new versions by itself — just press `y` when prompted. To update
manually, quit with `/exit` and run:

Confirm with `meshapi --version`. See
[Staying up to date](/cli/commands#staying-up-to-date) for details.

## It won’t write files or run commands automatically

That’s the **permission mode**, shown in the colored bar at the bottom.

- **default** (green) asks before every file write, command, or search — that’s why it keeps prompting.
- Press **Shift+Tab** to cycle to**accept edits** ,**auto** , or**bypass permissions** for less prompting.
- Even in **bypass permissions** , genuinely dangerous actions (`rm -rf` ,`sudo` , writing to`~/.ssh` ) still stop and ask. See[Commands & permission modes](/cli/commands#permission-modes) .

## What do the y / a / n prompts mean?

When the CLI wants to touch your computer it asks to **approve tool call**:

- **`y`** — allow this once.
- **`a`** — allow and don’t ask again for**this tool** for the rest of the session.
- **`n`** — deny.

If you approved with `a` and want prompts back, restart the CLI or switch to a stricter permission mode.

## ‘No models match’ or /model won’t switch

- Type `/model` then start typing — fuzzy suggestions appear; pick with arrows + Enter, or type the full ID (e.g.`openai/gpt-4o-mini` ).
- `/models` browses the full catalog with prices;`/models <filter>` narrows it. A filter matching nothing prints`No models match '<filter>'` — try a broader term.
- Prefer not to choose? `/route auto` lets Mesh pick per prompt (see[Auto Routing](/auto-routing) ).

## Pasting the key or text doesn’t work (Windows)

In PowerShell, paste with **right-click**, not `⌘/Ctrl+V`. When pasting your
API key on first launch, **nothing shows on screen** — it’s hidden for
security. Type your input and press Enter.

## /memory or /cost look wrong

- `/memory` shows what the CLI has learned about the**current project** — it’s per-folder, so running from a different directory shows different memory.
- `/cost` is spend for the**current session** only; for full history use Dashboard →**Logs** .

## Connection / network errors

The CLI talks to `https://api.meshapi.ai`. Behind a corporate proxy or VPN,
make sure that host is reachable. A `402` inside the CLI is a balance issue, not
a network one — see the balance entry above.

## Still stuck?

Email **[contact@meshapi.ai](mailto:contact@meshapi.ai)** with the command you ran, the full output, and
your CLI version (`meshapi --version`). More options on the [Support](/support)
page.

# Citations

1. Source page: https://developers.meshapi.ai/debug/mesh-cli
