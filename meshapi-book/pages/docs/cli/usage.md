---
type: Web Page
title: Using the Mesh CLI - Mesh API
description: Chat, switch models, and run code from your terminal with the Mesh CLI.
resource: https://developers.meshapi.ai/docs/cli/usage
timestamp: '2026-08-10T07:50:31.317333+00:00'
---

[installed](/docs/cli/install), you’re one API key away from your first response. Commands are identical on macOS and Windows — the only difference is how you paste (

**⌘+V**on Mac,

**right-click**in PowerShell).

## 1. Get your API key

This is a one-time step.
1. Go to [app.meshapi.ai](https://app.meshapi.ai) and sign in (or create an account).
2. In the left menu, click **API Keys → Create key** and name it anything.
3. Click the copy icon. The key starts with `rsk_` . Keep it private — treat it like a password.

## 2. First launch

1. In Terminal (Mac) or PowerShell (Windows), run:

1. It asks for your key once. Paste it and press **Enter** .

Nothing appears on screen while you paste the key — that’s normal, it’s hidden
for security.

1. You land on the home screen, which shows your **version** , current folder (**cwd** ), and the active**model** :

## 3. Say hello

Type a message and press
**Enter**:

**model used**,

**tokens in→out**,

**cost**, and

**time**:

## 4. Switch models

Over 1000 models are available.
- Type `/model` and start typing a name — a suggestion menu pops up as you type (e.g.`qw` shows all Qwen models). Pick with the arrow keys +**Enter** .
- Or type it in full:

- To browse the full catalog with prices, type `/models` (or filter, e.g.`/models claude` ). Prices are shown**per 1 million tokens** .

## 5. Let it write and run code

The CLI can create and run files for you. Try:
**asks permission**and shows exactly what it wants to do:

- **`y`** — yes, this once.
- **`a`** — yes, and don’t ask again for this tool for the rest of the session.
- **`n`** — no.

`y` to write the file, then `y` again when it asks to run it. The CLI
runs the file and shows the output:
**permission mode**— see

[Commands & permission modes](/docs/cli/commands#permission-modes).

## Next step

Learn the
[commands and permission modes](/docs/cli/commands)you’ll use every day.

# Citations

1. Source page: https://developers.meshapi.ai/docs/cli/usage
