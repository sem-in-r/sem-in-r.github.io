---
name: preview
description: Render and serve the site with live reload via `quarto preview`, and open it in Chrome. Use to preview the site or check changes in the browser.
allowed-tools: Bash, mcp__claude-in-chrome__tabs_context_mcp, mcp__claude-in-chrome__navigate, mcp__claude-in-chrome__computer
---

# Preview Site

Run `quarto preview` to render the site and serve it with **live reload**, then
open it in Chrome. This is a single step: Quarto renders, serves, and watches the
source — editing any `.qmd`/`custom.scss` re-renders and refreshes the browser
automatically. **You do not need to run `/build` first**, and you do not re-run
this skill after each edit; just save and the open tab updates.

## Instructions

1. Start (or restart) the preview server, in the background so it doesn't block.
   Kill any existing `quarto preview` first so the fixed port is free:
   ```bash
   pkill -f 'quarto preview' 2>/dev/null; sleep 1
   cd /Users/soumyaray/Sync/Dropbox/ossdev/rpackages/sem-in-r/seminr.github.io && \
     quarto preview --port 4321 --no-browser --host localhost
   ```
   Wait for the line `Browse at http://localhost:4321/` in the output before
   continuing. If port 4321 is reported in use by something else, pick another
   port (e.g. 4331) and use that URL below.

2. **Open the preview in Chrome — this is a required part of the skill, not
   optional.** Every invocation of `/preview` must end with Chrome sitting on the
   served page:
   - Call `tabs_context_mcp` with `createIfEmpty: true` to get or create a tab.
     **Reuse the existing MCP tab if one is already open** (don't spawn a new tab
     on every invocation); only create one if none exists.
   - Navigate that tab to the served URL (`http://localhost:4321`). If the user
     asked to preview a specific page, navigate directly to that page's URL
     (e.g. `http://localhost:4321/resources.html`) rather than the home page.
   - Take a screenshot to confirm the preview loaded.

## Notes

- **Always leave Chrome open on the right page.** If the server is already
  running and Chrome is already on the URL, just re-navigate/refresh the existing
  tab — but never finish `/preview` without Chrome pointed at the served page.
- `quarto preview` renders to `_build/docs/` (the `output-dir`) and watches for
  changes. The server runs in the background — use `/tasks` to see and stop it.
- **Never `cd` into `_build/`** (it's the `build`-branch repo). Run `quarto
  preview` from the project root; Quarto writing output there is fine.
- To publish what you see, run `/build` (or rely on the render preview already
  produced) and then `/publish`.
