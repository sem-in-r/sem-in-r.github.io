---
name: build
description: Render the Distill site. Use when asked to build, rebuild, or render the site.
disable-model-invocation: true
allowed-tools: Bash
---

# Build Site

Render the full Distill site by running:

```r
Rscript -e 'rmarkdown::render_site()'
```

Report any errors or warnings to the user after the build completes.
