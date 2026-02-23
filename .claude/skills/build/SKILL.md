---
name: build
description: Render the Distill site. Use when asked to build, rebuild, or render the site.
disable-model-invocation: true
allowed-tools: Bash
---

# Build Site

Render the full Distill site. Always run from the project root (not from `_build/`):

```bash
cd /Users/soumyaray/Sync/Dropbox/ossdev/rpackages/sem-in-r/website-seminr && Rscript -e 'rmarkdown::render_site()'
```

After the build completes, ensure the working directory is back at the project root. Report any errors or warnings to the user.
