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

**Important:** Any `.md` or `.Rmd` file in the project root (outside of dotfolders like `.claude/` or underscore-prefixed dirs like `_posts/`) will be rendered into HTML by `rmarkdown::render_site()`. Keep non-site markdown files inside dotfolders to avoid unwanted build output.

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

## Skills

The following skills are available and should be suggested when appropriate:

| Skill | When to suggest |
|-------|-----------------|
| `/build` | User asks to build, render, or rebuild the site |
| `/publish` | User asks to publish, deploy, or push the built site to GitHub Pages |

**When the user asks to "commit", "push", or "commit and push":** this almost always refers to the **`main`** branch (source files). If the context suggests the user may want to publish the rendered site instead (e.g., they just ran `/build`, or they mention the live site), **ask whether they meant to use `/publish`** before proceeding.

## Git Workflow

This repo uses two branches that correspond to two separate git checkouts:

| Branch | Directory | Contents |
|--------|-----------|----------|
| **`main`** | Project root (`.`) | Source `.Rmd` files, config, skills — the working branch |
| **`build`** | `_build/` | Rendered HTML output served by GitHub Pages |

The `_build/` folder is a **separate git repo** checked out on the `build` branch of the same remote. It is gitignored by `main`.

- Branch from `main` for changes and submit PRs back.
- Use `git` (or `git -C .`) for the source repo on `main`.
- Use `git -C _build` for the rendered-output repo on `build` — never `cd` into `_build/`.
- Do **not** include `_build/` in PRs — the maintainer re-renders after merge.
