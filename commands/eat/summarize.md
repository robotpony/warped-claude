---
name: eat:summarize
description: Summarize recipes created on a given date, with Obsidian links
argument-hint: [YYYY-MM-DD] [vault-folder, e.g. "food"]
---

Arguments: $ARGUMENTS

Parse arguments: the first token is an optional date (defaults to today). Everything after is an optional vault-relative folder path to search (defaults to the food notes directory).

## Steps

**1. Find recipes.** Locate all `.md` files in the target folder created or modified on the given date. Use `find` with `-newer` or check `ls -lt` to identify candidates, then confirm by reading the `date:` frontmatter.

**2. Read each recipe.** Extract from frontmatter and body:
- `title`
- `cuisine` and primary type tag
- `servings`, `total_time`
- `tags`
- The description paragraph (if present)
- Key characteristics from ingredients and method (protein, main veg, cooking style)
- Any notable notes or tips

**3. Build the Obsidian URL** for each recipe:
- Format: `obsidian://open?vault=<vault-name>&file=<relative-path-no-extension>`
- The vault name is the root folder name (e.g. `development-notes`)
- The file path is relative to the vault root (e.g. `food/chicken-yakisoba`)
- URL-encode spaces as `%20` if needed

**4. Output the summary** as a markdown list or table. For each recipe include:
- Title as a clickable Obsidian link
- Cuisine, time, servings
- Tags from frontmatter rendered as hashtags (e.g. `#weeknight #low-carb`)
- 1–2 sentence description of what the dish is and why it's worth making
- Any standout notes (substitutions, tips)
