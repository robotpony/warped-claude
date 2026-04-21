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
## <Title>

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

- **Cut, don't add.** Remove filler, deduplicate, and tighten language. Do not introduce scope, questions, or signals that aren't in the source material.
- **Preserve all decisions and constraints** from the original. If the source says "global COGS % only," keep that constraint.
- **Open questions must be specific.** Each should name what would change depending on the answer. Drop questions that are really statements.
- **No em-dashes** in prose (use commas, semicolons, or colons).
- **Canadian English** spelling (colour, behaviour, organize, optimize).
- **No H1 headers.** Start with H2.
- **No YAML front-matter** in the output.

## Output

4. Print the rewritten ticket as markdown text.
5. Do not save to a file unless the user asks.
6. If the rewrite dropped significant content from the original (entire sections, specific requirements), note what was cut and why at the end.
