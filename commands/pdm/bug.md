---
name: pdm:bug
description: Write a structured bug report from a Slack thread, screenshot, or description and file it in the vault
argument-hint: <paste Slack thread, describe the bug, or provide a file path>
---

Write a structured bug report from user-provided input (Slack thread, screenshots, description, or a combination) and file it in the Obsidian vault.

## Input

1. Accept $ARGUMENTS as the bug source. This may be:
   - A pasted Slack thread or message
   - A path to a screenshot or set of screenshots
   - A free-text description
   - A combination of the above
2. If screenshots are provided, read them to understand the current UI/behaviour.
3. Extract: **reporter** (who flagged it), **date** (when), **area** (which product surface), and **what's wrong**.

## Clarify

4. If severity is not obvious from context, ask the user:
   - **Data bug**: incorrect calculations, wrong values, data loss
   - **UX/labeling**: confusing UI, unclear copy, input ambiguity
   - **Functional**: broken feature, error state, crash
   - **Visual**: styling, layout, cosmetic issues
5. If the product area is ambiguous, ask which surface this affects (e.g., Benchmarks, Onboarding, Dashboard, Settings).

## Write the bug report

6. Create `bugs/<slug>.md` where `<slug>` is a short kebab-case description (e.g., `cogs-per-order-vs-per-unit-ambiguity`).

7. Use this structure:

```
**Source:** <Reporter name> (<channel/context>, <date>)
**Severity:** <type> — <one-line characterization>
**Area:** <product surface> > <specific page or component>
**Status:** Open

## Problem

<2-3 sentences: what's wrong and why it matters. Lead with user impact, not implementation detail.>

## What it looks like today

<Describe or sketch the current behaviour. Use an ASCII layout sketch if a UI element is involved. Reference screenshots if provided.>

**Problems:**
<Numbered list of specific issues observed>

## Competitor reference

<If the reporter or context includes a comparison to another product, summarize it here. Note if screenshots are available or need to be requested.>

## Proposed fix

**Minimum (<priority>, low effort):**
<Smallest change that addresses the core confusion or bug>

**Better (<priority>, moderate effort):**
<More complete fix with better UX>

**Best (<priority>, more effort):**
<Ideal solution if we had time>

## Impact

<How this affects users, CS, data quality, or downstream metrics. Connect to known themes if relevant (e.g., "benchmarks dissatisfaction from CS sync").>

## Open questions

<Bulleted list of things to confirm before or during implementation>
```

## Formatting rules

- ASCII layout sketches follow the rules in `~/.claude/rules/product-writing-rules.md` (indentation for hierarchy, dot leaders, no box-drawing characters)
- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Use `[[wikilinks]]` for internal vault references
- Keep proposed fixes concrete and scoped, not aspirational

## Link into the weekly log

8. Find the current weekly log in `log/2026/`.
9. Add a brief entry under the current or most recent day section:

```
### Bug: <short title>

<Reporter> flagged <one-line summary>. Wrote up as [[<slug>]]. <One sentence on the recommended minimum fix.>
```

10. If the bug maps to an existing task section in the weekly log (e.g., "Benchmarks", "Onboarding"), mention the connection.

## After filing

11. Ask the user: "Want me to create a ClickUp ticket for this?"
    - If yes, use the bug report content to create a task with appropriate priority and tags.
12. Report the file path and weekly log link.
