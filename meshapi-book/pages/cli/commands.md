---
type: Web Page
title: Commands & permission modes | Mesh API Docs
description: The slash commands you'll use daily, the four permission modes, and how
  to stay up to date.
resource: https://developers.meshapi.ai/cli/commands
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

Commands & permission modes

Commands & permission modes

## Permission modes

The colored bar at the bottom of the screen always shows the current permission
mode. Press **Shift+Tab** to cycle through the four modes:

Even in **bypass permissions** mode, genuinely dangerous actions — `rm -rf`,
`sudo`, or writing to `~/.ssh` — still stop and ask for confirmation.

## Commands you’ll actually use

Slash commands work from the prompt inside the CLI:

## Staying up to date

New versions add models, fix bugs, and expand what the CLI can do. You don’t reinstall — you update.

**Easiest:** the CLI checks for new versions itself. When it offers one, press
`y`. To check any time while it’s running, type `/update`.

**Manual:** quit the CLI with `/exit`, then run this (same on Mac and Windows):

Confirm it worked:

Check that the number went up. If it still shows the old version, close the window, open a new one, and check again.

Running into errors? See [Mesh CLI debugging](/debug/mesh-cli).

# Citations

1. Source page: https://developers.meshapi.ai/cli/commands
