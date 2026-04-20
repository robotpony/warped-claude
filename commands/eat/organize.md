---
name: eat:organize
description: Build an organized index of all recipes in the vault, grouped by type
argument-hint: [output-file, e.g. "food/Recipe Index.md"]
---

Arguments: $ARGUMENTS

Parse arguments: the first token is an optional output file path relative to the vault root. If omitted, output to stdout only.

## Steps

**1. Find all recipes.** Search the vault for `.md` files that have a recipe-style frontmatter — specifically a `tags:` field containing at least one of: `mains`, `sides`, `soups`, `salads`, `desserts`, `drinks`, `breads`, `sauces`, `snacks`. Exclude:
- `gdrive/` directory
- `_templates/` directory
- `log/` directory
- Files without a `title:` frontmatter field

Use `grep -rl` across the vault root to find candidates, then read each to confirm.

**2. Parse each recipe.** Extract:
- `title`
- `tags` (full list)
- `cuisine`
- `total_time`
- `servings`
- File path relative to vault root (for Obsidian URL construction)

**3. Build the Obsidian URL** for each recipe:
- Format: `obsidian://open?vault=<vault-name>&file=<relative-path-no-extension>`
- Vault name = root folder name (e.g. `development-notes`)
- URL-encode spaces as `%20`

**4. Group recipes** by their primary type tag in this order:
`mains`, `breads`, `sides`, `soups`, `salads`, `sauces`, `snacks`, `desserts`, `drinks`

Within each group, sort alphabetically by title.

**5. Output the index** as a markdown document with:
- A top-level count: "N recipes"
- One `##` section per type group (only include groups that have recipes)
- Each recipe as a bullet: `- [Title](obsidian-url) — Cuisine · Time · Tags as #hashtags`
- A footer with the date generated

**6. Write or print.**
- If an output file path was given: write to `<vault-root>/<output-file>`. Confirm with the path written.
- If no path given: print the markdown to stdout.
