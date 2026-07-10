# SEMinR Website Repository

This is the repository for the SEMinR website, intended for SEMinR authors, maintainers, and collaborators to design and maintain the SEMinR website. The site is built with [Quarto](https://quarto.org) and deployed via GitHub Pages.

## Setup

**Clone this repo:** Get the website repo from GitHub.

```shell
git clone git@github.com:sem-in-r/sem-in-r.github.io.git
```

**Install Quarto:** Download and install the Quarto CLI from
<https://quarto.org/docs/get-started/>. That is the only dependency needed to
render and preview the site — the site is static prose, code, and images (no R
execution required to build).

Verify it is installed:

```shell
quarto --version
```

The rendered site is written to a `gitignored` `_build/docs/` folder for local
preview; Quarto creates it automatically on the first render.

## Working with Git

**Important branches:**

- `main` - major code branch where all changes to the site source code are made
- `build` - where all rendered content is placed (do not edit or push this branch directly); note that you will not find the `build` branch in your normal checkout — only the website manager maintains it (it is a separate checkout under `_build/`).

**Working with branches:** Please branch out of `main` and work in your own branch only. Create your articles/posts and submit a Pull Request (PR) back to `main`. Do not include the `_build/` folder in your PRs — the website maintainer re-renders after merge.

## Rendering & Previewing the Website

**Live preview (recommended while writing):** render + serve with live reload in
one command. Editing any `.qmd` or the theme re-renders and refreshes the browser
automatically:

```shell
quarto preview
```

**One-shot render** of the whole site to `_build/docs/`:

```shell
quarto render
```

(Claude Code users: the `/preview` and `/build` skills wrap these.)

## Creating New Blog Posts

*See any existing post under `posts/` (e.g. `posts/2021-05-26-seminr-20-released/`) as a model.*

Posts live in dated folders under `posts/`, each with an `index.qmd`. The News
listing (`posts.qmd`) auto-discovers them — there is no per-post build step, and
no listing to hand-edit.

**Create a new post:** make a dated folder and a starter `index.qmd`. Claude Code
users can run the **`/new-post`** skill, which scaffolds this given a title.
Manually:

```shell
mkdir -p posts/2026-07-10-my-post-title/images
```

Then create `posts/2026-07-10-my-post-title/index.qmd` with frontmatter:

```yaml
---
title: "My Post Title"
description: |
  One or two sentences summarizing the post — shown on the News listing.
author:
  - name: Your Name
    url: https://your-url.example
date: 2026-07-10
image: /images/seminr_logos/seminr-logo.png
---
```

**Edit your post:**

- Write the body in Markdown below the frontmatter. Use native Quarto features —
  e.g. `::: {.panel-tabset}` for language-switcher code tabs.
- `image:` is the listing thumbnail. The default above is the landscape SEMinR
  wordmark (the site default when a post has no image of its own). For post
  specific art, put files in the post's `images/` folder and set
  `image: images/your-thumbnail.png`.
- Use the post's `images/` folder for all other images in the post too.
- Run `quarto preview` (or `/preview`) to see it live; the post appears on the
  News listing automatically, newest first.
