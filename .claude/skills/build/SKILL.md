---
name: build
description: Render the site with Quarto. Use when asked to build, rebuild, or render the site.
allowed-tools: Bash
---

# Build Site

Render the full site with Quarto. Always run from the project root (not from `_build/`):

```bash
cd /Users/soumyaray/Sync/Dropbox/ossdev/rpackages/sem-in-r/seminr.github.io && \
  quarto render
```

Output goes to `_build/docs/` (set by `output-dir` in `_quarto.yml`), which is
the separate `build`-branch checkout that GitHub Pages serves. `CNAME` and
`.nojekyll` are copied into the output automatically via the `resources:` key.

No post-build cleanup is needed: `_quarto.yml`'s `project: render:` list renders
only the site's `*.qmd` pages and `posts/`, so root markdown that isn't site
content — `README.md` and the private `CLAUDE.local.md` — is never rendered.

After the build completes, ensure the working directory is back at the project
root. Report any errors or warnings to the user.

## Notes

- **Never `cd` into `_build/`** (it's the `build`-branch repo). Quarto writing to
  it via `output-dir` from the project root is fine — do not change into it.
- To preview with live reload instead of a one-shot render, use `/preview`
  (`quarto preview`), which renders and serves in one step.
