---
name: publish
description: Publish the rendered site by committing _build/docs to the build branch and pushing. Use after building the site.
allowed-tools: Bash
---

# Publish Site

Publish the rendered site from `_build/docs/` to the `build` branch and push to GitHub Pages.

## Steps

1. **Check for changes** in the `_build` repo:
   ```bash
   git -C _build status
   ```
   If there are no changes (nothing to commit), inform the user and stop.

2. **Show the user a summary** of what changed using `git -C _build diff --stat` and list the key changes (modified pages, added/removed libs, etc.). Ask the user to confirm before committing.

3. **Stage and commit** all changes:
   ```bash
   git -C _build add -A
   git -C _build commit -m "<descriptive commit message>"
   ```
   Write a concise commit message summarizing the changes.

4. **Pull remote changes** (the remote may have commits like CNAME that aren't local):
   ```bash
   git -C _build fetch origin build
   git -C _build pull --rebase origin build
   ```

5. **Push** to the remote:
   ```bash
   git -C _build push origin build
   ```

6. Report success or any errors to the user.

## Important

- The `_build/` directory is a separate git repo on the `build` branch.
- Use `git -C _build` for all git commands (do not change the working directory).
- Never force-push.
- Do not include co-author lines in commit messages.
