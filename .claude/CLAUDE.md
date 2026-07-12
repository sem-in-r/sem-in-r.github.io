# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the website for the [SEMinR](https://seminr.io/) R package, built with [Quarto](https://quarto.org). It produces a static site deployed via GitHub Pages.

## Related Repositories

This website documents SEMinR and may reflect or refer to elements (API, syntax,
features, examples) from the SEMinR source repositories, which are sibling
directories of this one:

| Repo | Path (relative to this site root) | Role |
|------|-----------------------------------|------|
| **`seminr`** | `../seminr` | The **base** implementation — the original R package |
| **`seminr-py`** | `../seminr-py` | Python port of SEMinR |
| **`seminr-ts`** | `../seminr-ts` | TypeScript port of SEMinR |

The R version (`../seminr`) is authoritative; `seminr-py` and `seminr-ts` are
ports to other languages. When site content describes SEMinR's behavior, syntax,
or features, cross-check against these repos (starting with the R base) rather
than assuming.

## Build Commands

**Install dependencies:** the only dependency is the [Quarto CLI](https://quarto.org/docs/get-started/). No R execution is needed to build — the site is static prose, code, and images.

**Render full site:**
```bash
quarto render
```
Output goes to `_build/docs/` (gitignored), set by `output-dir` in `_quarto.yml`. Prefer the **`/build`** skill, which wraps this.

**Preview with live reload:**
```bash
quarto preview
```
Renders, serves, and watches the source in one step — editing any `.qmd` or the theme re-renders and refreshes the browser automatically. Prefer the **`/preview`** skill (it also opens Chrome to the page). There is **no separate build-then-serve step**: just save and the open tab updates.

**Create a new blog post:** use the **`/new-post`** skill (Quarto has no built-in post scaffolder), or follow the **"Creating New Blog Posts"** section of `README.md`. It scaffolds a dated `posts/YYYY-MM-DD-slug/index.qmd`. There is no per-post render step — `quarto render`/`quarto preview` discover new posts automatically and the News listing picks them up.

**Render scope:** `_quarto.yml`'s `project: render:` list renders only the site's `*.qmd` pages and `posts/`, so root markdown that isn't site content — `README.md` and the private `CLAUDE.local.md` — is never rendered into the output.

### Gotchas (learned the hard way)

- **Never run `quarto render` while `quarto preview` is live.** They share the
  same output dir and SASS cache (Quarto's Deno KV store). A manual render
  underneath a running preview corrupts that cache, and the preview then throws
  `Bad resource ID` (`SassCache.getFromHash` in the log) on every page. Fix:
  `pkill -f 'quarto preview'` and restart it. So while previewing, **just save
  and let live-reload re-render** — don't invoke `quarto render` yourself. If you
  need a one-off standalone render (e.g. to grep the HTML), stop the preview
  first.

- **Extensionless image URLs get `.png` appended by Pandoc.** Quarto's
  `default-image-extension: png` appends `.png` to any image whose path has no
  file extension. For a shields.io badge like
  `https://img.shields.io/cran/v/seminr?...&style=flat-square` this yields
  `...style=flat-square.png`, silently breaking the query string. Fix: give the
  URL an explicit format extension **in the path, before the `?`** —
  `https://img.shields.io/cran/v/seminr.svg?...`. shields serves SVG for the
  `.svg` suffix, and Pandoc leaves a URL with an extension alone. The version
  badges on `packages.qmd`/`extras.qmd` rely on this — keep the `.svg`.

## Architecture

- **`_quarto.yml`** — Main site config: project type/`output-dir` (`_build/docs`), `render:` list, `resources:` (CNAME, .nojekyll), navbar, theme, `site-url`, favicon, `execute: freeze`
- **`index.qmd`** — Homepage with SEMinR introduction and code examples (includes a one-line script that enables the homepage's larger "hero" navbar)
- **`packages.qmd`, `extras.qmd`, `resources.qmd`, `community.qmd`** — top-level content pages (Community is the team/authors page)
- **`posts.qmd`** — Blog listing page (Quarto `listing:` — auto-discovers posts under `posts/`)
- **`posts/`** — Blog posts, each in a dated folder (`YYYY-MM-DD-slug/index.qmd`)
- **`images/`** — Shared site images (logos, homepage diagram)

### Styling (`custom.scss`)

Site CSS lives in **`custom.scss`**, a Quarto SCSS theme layer wired into
`_quarto.yml` under `format: html: theme: [cosmo, custom.scss]`. Listing it in
`theme:` applies it **site-wide, including blog posts**, so there is **no need
to inline post-specific CSS**. Language-switcher code tabs use the native
`::: {.panel-tabset}`, and the post byline is a restyled native title block.

The file has `/*-- scss:defaults --*/` (SCSS variables like `$grid-body-width`,
`$primary`) and `/*-- scss:rules --*/` (component CSS). **Editing `custom.scss`
requires a re-render (`/build` or a running `quarto preview`) for changes to
appear.** With `quarto preview` running, saves re-render and refresh the browser
automatically.

### Blog Post Structure

Each post lives in `posts/YYYY-MM-DD-slug/` containing:
- `index.qmd` — Post source with YAML frontmatter (`title`, `description`, `author`, `date` in ISO `YYYY-MM-DD`, `image:` thumbnail)
- `images/` — Post-specific images (including the thumbnail referenced by `image:` in frontmatter)

The dated folder name is the post's URL (`/posts/YYYY-MM-DD-slug/`); keep it stable to preserve links. Listing order is driven by the `date:` field.

**Default post image:** When a post has no image of its own, use the **landscape SEMinR wordmark** (`images/seminr_logos/seminr-logo.png`).

## Deployment

- **Domain:** The site is registered at **seminr.io**, which points to the GitHub Pages deployment.

## Skills

The following skills are available and should be suggested when appropriate:

| Skill | When to suggest |
|-------|-----------------|
| `/build` | User asks to build, render, or rebuild the site (`quarto render`) |
| `/preview` | User asks to preview the site or view changes in the browser (`quarto preview` + Chrome) |
| `/publish` | User asks to publish, deploy, or push the built site to GitHub Pages |
| `/new-post` | User asks to create/start a new blog post or news item |

**When the user asks to "commit", "push", or "commit and push":** this almost always refers to the **`main`** branch (source files). If the context suggests the user may want to publish the rendered site instead (e.g., they just ran `/build`, or they mention the live site), **ask whether they meant to use `/publish`** before proceeding.

## Git Workflow

This repo uses two branches that correspond to two separate git checkouts:

| Branch | Directory | Contents |
|--------|-----------|----------|
| **`main`** | Project root (`.`) | Source `.qmd` files, `_quarto.yml`, `custom.scss`, skills — the working branch |
| **`build`** | `_build/` | Rendered HTML output served by GitHub Pages |

The `_build/` folder is a **separate git repo** checked out on the `build` branch of the same remote. It is gitignored by `main`.

- Branch from `main` for changes and submit PRs back.
- Use `git` (or `git -C .`) for the source repo on `main`.
- Use `git -C _build` for the rendered-output repo on `build` — never `cd` into `_build/`.
- Do **not** include `_build/` in PRs — the maintainer re-renders after merge.
