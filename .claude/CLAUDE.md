# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the website for the [SEMinR](https://seminr.io/) R package, built with [Distill for R Markdown](https://rstudio.github.io/distill/). It produces a static site deployed via GitHub Pages.

## Build Commands

**Install dependencies (R console):**
```r
install.packages('distill')
```

**Render full site (R console):**
```r
rmarkdown::render_site()
```

**Create a new blog post (R console):**
```r
distill::create_post("Title of your post")
```

**Knit a single post:** Open the post's `.Rmd` file and knit it. Distill caches rendered posts and won't re-render them during a full site build.

Output goes to `_build/docs/` (gitignored).

## Architecture

- **`_site.yml`** — Main site config: navbar, output directory (`_build/docs`), base URL, cookie consent
- **`index.Rmd`** — Homepage with SEMinR introduction and code examples
- **`about.Rmd`** — Team/authors page
- **`posts.Rmd`** — Blog listing page (uses `listing: posts` in frontmatter)
- **`_posts/`** — Blog posts, each in a dated folder (`YYYY-MM-DD-slug/`)
- **`images/`** — Shared site images (logo)

### Blog Post Structure

Each post lives in `_posts/YYYY-MM-DD-slug-title/` containing:
- `slug-title.Rmd` — Post source with YAML frontmatter (title, description, author, date, preview image)
- `images/` — Post-specific images (including thumbnail referenced by `preview:` in frontmatter)
- Generated `_files/` directory and `.html` output from knitting

## Deployment

- **Domain:** The site is registered at **seminr.io**, which points to the GitHub Pages deployment.

## Git Workflow

- **`main`** — Source branch; branch from here for changes and submit PRs back
- **`build`** — Rendered content managed only by the website maintainer (gitignored locally)
- Do **not** include `_build/` in PRs — the maintainer re-renders after merge
