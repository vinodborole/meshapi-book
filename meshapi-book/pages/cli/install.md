---
type: Web Page
title: Install the Mesh CLI | Mesh API Docs
description: Get the Mesh CLI running on macOS or Windows in about 10 minutes.
resource: https://developers.meshapi.ai/cli/install
timestamp: '2026-07-09T12:17:20.455852+00:00'
---

# Install the Mesh CLI

The Mesh CLI (`meshapi`) is a terminal app that lets you chat with any model,
switch between 1000+ models, and have it write and run code for you — all from
your Terminal or PowerShell.

It installs with [pipx](https://pipx.pypa.io/), which needs Python. The steps
below install Python, pipx, and the CLI from scratch. Pick your OS.

The whole install is three tools: **Python** → **pipx** → **meshapi-code**.
After the one-time setup, updating is a single command — see
[Using the CLI → Staying up to date](/cli/commands#staying-up-to-date).

###### macOS

###### Windows

### 1. Install Python

- Open Safari (or any browser) and go to [python.org/downloads](https://python.org/downloads).
- Click the yellow **Download Python 3.x**button (any recent 3.x is fine).
- Open the downloaded file — named like `python-3.14.6-macos11.pkg`.
- Click **Continue → Continue → Continue → Agree → Install**, enter your Mac login password, then**Install Software**.
- Wait for *“The installation was successful”*and click**Close**.

### 2. Check Python

- Press **⌘ + Space**, type`Terminal`, press**Enter**.
- Run:

You should see `Python 3.` followed by numbers. ✅

### 3. Install pipx

The second command prints something like `Success! Added /Users/you/.local/bin to the PATH`.

Now **quit Terminal completely** (**⌘ + Q**) and open a fresh window
(**⌘ + Space → Terminal**). The next command only works in a new window that
has picked up the updated PATH.

### 4. Install the Mesh CLI

When you see `done!` and `installed package meshapi-code`, the CLI is ready. ✅

## Next step

The CLI is installed. Head to [Using the Mesh CLI](/cli/usage) to add your API
key and send your first prompt.

Hit a snag during install? See [Mesh CLI debugging](/debug/mesh-cli).

# Citations

1. Source page: https://developers.meshapi.ai/cli/install
