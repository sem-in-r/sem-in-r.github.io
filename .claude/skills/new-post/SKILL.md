---
name: new-post
description: Scaffold a new blog post (dated folder + starter index.qmd with frontmatter). Use when asked to create/start a new post, blog entry, or news item.
allowed-tools: Bash, Write
---

# New Blog Post

Quarto has no built-in new-post command, so this skill scaffolds a new post by
hand. Given a **title** (ask for one if not provided), create a dated
post folder and a starter `index.qmd`, ready to write. Rendering and listing
pickup are automatic — `quarto render` / `quarto preview` discover the new post
and it appears on the News listing (`posts.qmd`) in reverse-chronological order.
There is no per-post build step.

## Steps

1. **Derive the folder name.** Use today's date and a slug from the title:
   ```bash
   DATE=$(date +%F)                     # YYYY-MM-DD
   # slug: lowercase, spaces/punctuation → hyphens, collapse repeats, trim
   SLUG=$(echo "TITLE HERE" | tr '[:upper:]' '[:lower:]' \
          | sed -E 's/[^a-z0-9]+/-/g; s/^-+|-+$//g')
   echo "posts/${DATE}-${SLUG}"
   ```
   The dated-folder convention (`posts/YYYY-MM-DD-slug/`) matches every existing
   post and keeps URLs stable and self-sorting.

2. **Create the folder and its `images/` subfolder:**
   ```bash
   mkdir -p "posts/${DATE}-${SLUG}/images"
   ```

3. **Write `posts/${DATE}-${SLUG}/index.qmd`** with this starter frontmatter
   (fill in the description; adjust author; set `date` to `$DATE`):

   ```yaml
   ---
   title: "<Title>"
   description: |
     <One or two sentences summarizing the post — shown on the News listing.>
   author:
     - name: Soumya Ray
       url: https://soumyaray.com
   date: <YYYY-MM-DD>
   image: /images/seminr_logos/seminr-logo.png
   ---

   <!-- Write the post here. Use native Quarto features:
        - code tabs:  ::: {.panel-tabset} ... :::
        - images:     put files in this folder's images/ and reference
                      them as images/your-file.png -->
   ```

   - **`image:`** is the listing thumbnail. The default above is the **landscape
     SEMinR wordmark** (the site-wide default when a post has no image of its
     own — note: the landscape wordmark `seminr-logo.png`, not the square
     `logo.png`). When the post has its own art, drop it in this folder's
     `images/` and change `image:` to `images/your-file.png`.
   - **`author:`** defaults to Soumya Ray; change the name/url for other authors
     (e.g. Nicholas Danks — https://nicholasdanks.com).

4. **Tell the user** the file was created and how to preview it: run `/preview`
   (or, if already running, just save — it live-reloads and the post appears on
   the News listing automatically).

## Notes

- Don't hand-edit the listing — `posts.qmd` auto-discovers posts under `posts/`.
- Keep the folder name's date in sync with the `date:` frontmatter for tidy,
  predictable URLs, though only the `date:` field drives listing order.
