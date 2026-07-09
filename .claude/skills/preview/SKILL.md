---
name: preview
description: Start a local server for the rendered Distill site and open it in Chrome. Use when you want to preview the built site or check changes in the browser.
allowed-tools: Bash, mcp__claude-in-chrome__tabs_context_mcp, mcp__claude-in-chrome__navigate, mcp__claude-in-chrome__computer
---

# Preview Site

Serve the rendered site from `_build/docs/` and open it in Chrome for previewing.

This site is static output from `rmarkdown::render_site()`. Preview shows the **already-built** files in `_build/docs/`; it does not re-render. If the site hasn't been built yet (or you changed a `.Rmd` and want to see it), run `/build` first.

## Instructions

1. Confirm there is something to serve:
   ```bash
   ls _build/docs/index.html
   ```
   If it's missing, tell the user to run `/build` first and stop.

2. Start a static server on port 8080 rooted at `_build/docs/`, in the background so it doesn't block:
   ```bash
   npx live-server _build/docs --port=8080 --no-browser
   ```

3. Open the preview in Chrome:
   - Call `tabs_context_mcp` with `createIfEmpty: true` to get or create a tab
   - Navigate to `http://localhost:8080`
   - Take a screenshot to confirm the preview loaded

## Notes

- If a server is already running on port 8080, just navigate Chrome to the URL.
- The server runs in the background — use `/tasks` to see and stop it.
- **Never `cd` into `_build/`** (it's the `build`-branch repo). Serving it read-only from the project root, as above, is fine.
- Distill changes only appear after a re-render — edit the `.Rmd`, run `/build`, then refresh the browser.
