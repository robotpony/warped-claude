---
name: eat:pull-recipe
description: Fetches a recipe and formats it as markdown in a simple format
argument-hint: <URL> [vault-folder, e.g. "recipes/soups"]
---

Arguments: $ARGUMENTS

Parse arguments: the first token is the URL. Everything after it is an optional vault folder path (relative to the working directory of the caller). If no folder is given, output only — do not write a file.

## Steps

**1. Fetch the page.** Try the following in order, stopping at the first that succeeds:

1. **curl**: Run `curl -sL "<url>" -A "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"` via Bash, save to a temp file, then extract recipe content using python3 — parse JSON-LD `<script type="application/ld+json">` blocks for `@type: Recipe` data. If no JSON-LD found, extract raw text from `<main>` or `<article>` tags.
2. **claude-in-chrome**: If curl fails or returns no recipe data, load `mcp__claude-in-chrome__tabs_context_mcp` via ToolSearch, get a tab (create one if needed), navigate to the URL with `mcp__claude-in-chrome__navigate`, then extract content with `mcp__claude-in-chrome__get_page_text`.
3. **WebFetch**: If both fail, fall back to WebFetch.

Extract from whichever source succeeds: title, description/intro, ingredients (all groups), method steps, yield/servings, prep time, cook time, total time, cuisine, and dietary tags.

**2. Convert to the recipe format below.** Follow these rules precisely:

- All measurements MUST use metric units. Convert any imperial or US volumetric measures:
  - Volume: cups → ml (1 cup = 240 ml)
  - Weight: oz → g (1 oz = 28 g), lb → g or kg
  - Temperature: Fahrenheit → Celsius, keep °F in parentheses (e.g. `190°C (375°F)`)
  - Leave metric measures as-is
  - Round sensibly: 28.35 g → 30 g, 236 ml → 240 ml
- **Spoon measures** (teaspoon, tablespoon): keep as displacement — do NOT convert to ml. Write out the full word: `1 teaspoon`, `2 tablespoons`. Abbreviations like `tsp.` and `tbsp.` become full words.
- **Salt**: keep as written (weight or spoon measure). Never convert to ml.
- **Spices, dried herbs, and small solid seasonings** (pepper, chili flakes, cumin, paprika, etc.): keep as spoon measures or weight. Do not convert to ml.
- Strip all ads, SEO filler, author bios, and "jump to recipe" content
- Keep any useful tips, variations, or notes that are actually part of the recipe
- Use `source` frontmatter for the original URL
- Infer tags from context: one primary type tag (`#mains`, `#sides`, `#soups`, `#salads`, `#desserts`, `#drinks`, `#breads`, `#sauces`, `#snacks`) plus descriptive tags (`#vegetarian`, `#vegan`, `#gluten-free`, `#quick`, `#weeknight`, etc.)
- Set `date` to today: 2026-04-18

**3. Verify by fetching again.** Fetch the same URL a second time using the same method that succeeded. Cross-check:
- Are all ingredient groups present? (nothing dropped)
- Are step counts consistent?
- Are the metric conversions accurate?
- Note any discrepancies found and correct them silently

**4. Write or output the recipe.**

- Derive the filename from the recipe title: lowercase, spaces to hyphens, no special characters (e.g. `black-bean-tacos.md`).
- If a vault folder was given: write the file to `<vault-folder>/<filename>`. Confirm with the path written, e.g. `Written: recipes/soups/black-bean-tacos.md`.
- If no vault folder was given: output the markdown as a fenced code block (` ```markdown `), ready to paste into Obsidian.

---

## Format

```markdown
---
title: Recipe Name
tags: [#type, #descriptive]
source: https://original-url.com/recipe
date: YYYY-MM-DD
servings: 4
prep_time: 15 min
cook_time: 30 min
total_time: 45 min
cuisine: Italian
diet: [#vegetarian]
---

Optional one or two sentence description. Only include if the source has useful context about the dish — origin, what makes it distinctive, or when to make it.

## Ingredients

- 240 ml olive oil
- 2 cloves garlic, minced
- 400 g canned tomatoes

## Method

1. Heat oil in a large pan over medium heat.
2. Add garlic and cook until fragrant, about 1 minute.
3. Add tomatoes and simmer for 20 minutes.

## Notes

- Keeps in the fridge for up to 5 days.

## Variations

- **Spicy**: Add 1/2 tsp chili flakes with the garlic.
```

Omit `Notes` and `Variations` sections if they add nothing useful. Omit optional frontmatter fields if the source doesn't provide them. Use grouped ingredients (`### Dough`, `### Filling`, etc.) when the source organizes them that way.
