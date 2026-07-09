---
name: build
description: Render the Distill site. Use when asked to build, rebuild, or render the site.
allowed-tools: Bash
---

# Build Site

Render the full Distill site. Always run from the project root (not from `_build/`):

```bash
cd /Users/soumyaray/Sync/Dropbox/ossdev/rpackages/sem-in-r/seminr.github.io && \
  Rscript -e 'rmarkdown::render_site()' && \
  rm -rf _build/docs/CLAUDE.local.html _build/docs/CLAUDE.local_files
```

The trailing `rm` removes stray output that `render_site()` generates from the
private root `CLAUDE.local.md`. The site generator renders every non-underscore
root `.md` and offers no way to exclude one, so this file is produced on any
machine that has `CLAUDE.local.md` present; delete it after each build so it
never lingers in the output. (The `_build` repo also gitignores it as a second
line of defense, so it can never be published even if this step is skipped.)

After the build completes, ensure the working directory is back at the project root. Report any errors or warnings to the user.
