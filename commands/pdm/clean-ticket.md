---
name: pdm:clean-ticket
description: Rewrite a pasted or rough ticket into a clean, decision-ready format
argument-hint: <paste ticket text, provide a file path, or give a ClickUp task ID>
---

Rewrite a rough or verbose ticket into a clean, scannable format optimized for decision-making and engineering handoff.

## Input

1. Accept $ARGUMENTS as the ticket source. This may be:
   - Pasted ticket text (from ClickUp, Slack, a doc, etc.)
   - A file path to a vault note containing the ticket
   - A ClickUp task ID (use `clickup_get_task` to fetch it)
2. Extract: **title**, **motivation/signal**, **proposed solution**, **scope**, **exclusions**, and **open questions**.

## Rewrite the ticket

3. Use this structure:

```
## <Actionable title>

**Why:** <1-2 sentences: the customer or business signal driving this. Use specific numbers if available (e.g., "4-5 brands asked in the last two weeks"). Do not invent signals.>

**What:** <1-2 sentences: what we're building and the core value prop. State the outcome, not implementation detail.>

## v1 Scope

<Group by concern area using bold sub-headers. Bullet each concrete deliverable. Keep items specific enough to estimate.>

**<Area 1>**
- <deliverable>
- <deliverable>

**<Area 2>**
- <deliverable>

## Out of Scope

- <Explicit exclusion>
- <Explicit exclusion>

## Open Questions

1. **<Topic>:** <Specific question that would change the approach or scope if answered differently>
2. **<Topic>:** <question>
```

## Rules

- **Actionable title.** The title MUST start with a verb: Add, Remove, Investigate, Support, Surface, Enable, etc. (e.g. "Add gross profit metric via COGS," not "Gross Profit Metric (via COGS)").
- **Cut, don't add.** Remove filler, deduplicate, and tighten language. Do not introduce scope, questions, or signals that aren't in the source material.
- **Preserve all decisions and constraints** from the original. If the source says "global COGS % only," keep that constraint.
- **Open questions must be specific.** Each should name what would change depending on the answer. Drop questions that are really statements.
- **No em-dashes** in prose (use commas, semicolons, or colons).
- **Canadian English** spelling (colour, behaviour, organize, optimize).
- **No H1 headers.** Start with H2.
- **No YAML front-matter** in the output.

## Output

4. Print the rewritten ticket as markdown text.
5. If the rewrite dropped significant content from the original (entire sections, specific requirements), note what was cut and why at the end.

## Verify

6. Before saving, cross-check the rewritten ticket against available sources of truth:
   - **Glossary:** Read `Northbeam Glossary.md` to verify terminology (metric names, product terms, acronyms) is used correctly.
   - **Labeling ontology:** If the ticket involves channels, platforms, or UTMs, check `projects/Northbeam Default Labeling Ontology.md` for correct taxonomy.
   - **Roadmap:** Fetch the data source directly (`mcp__claude_ai_Notion__notion-fetch` on `collection://020011e4-1c05-42f7-8c5d-7ec61bd2adad`) to see if this work maps to an existing priority or is net-new. One call returns all entries — do not enumerate via `notion-search`. Do not use the deprecated gdrive copy.
   - **Style guide:** Check `gdrive/Guides and checklists/Northbeam Requirements Writing & Document Style Guide.md` for any naming or formatting conventions.
7. If verification surfaces a conflict (wrong term, duplicate roadmap item, misnamed concept), flag it inline with a note like `[Verify: glossary says X, source says Y]`.

## Save to vault

8. After printing the rewritten ticket, save it to `discussions/<slug>-<month>-<d>-<yyyy>.md` (e.g. `discussions/gross-profit-metric-april-21-2026.md`).
   - Use a short kebab-case slug derived from the title.
   - If the source includes tags or a customer name, add inline tags at the top of the file (e.g. `#astound #data-out`).
   - If references or source links exist in the original, preserve them in a `## References` section at the end.
9. Find the current weekly log in `log/2026/`.
10. Add a one-line link under the most relevant task section, or under the current day if no section fits:
    ```
    - [ ] Review [[<slug>]] and decide next steps #todo
    ```
11. Report the file path and the weekly log entry to the user.
