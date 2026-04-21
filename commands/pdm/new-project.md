---
name: pdm:new-project
description: Scaffold a new project outline in new-projects/ from a meeting note, Slack thread, or description
argument-hint: <describe the project idea, paste a Slack thread, or reference a vault note>
---

Create a project outline for an idea that isn't on the roadmap yet and needs definition before it can be prioritized.

## Input

1. Accept $ARGUMENTS as the source. This may be:
   - A free-text description of the idea
   - A pasted Slack thread or meeting excerpt
   - A reference to a vault note (e.g., `[[meeting-note]]`)
2. If a vault note is referenced, read it to extract relevant context.
3. Extract: **who proposed it**, **when**, **what it is**, and **why it matters**.

## Clarify

4. If the idea is too vague to outline, ask the user:
   - What problem does this solve for the customer?
   - Is there a known request or signal driving this? (CS feedback, customer call, internal pain)
5. If there's an obvious relationship to an existing roadmap item, note it but don't merge them.

## Write the outline

6. Generate a slug from the project idea (kebab-case, e.g., `landing-page-analytics`).
7. Check if `new-projects/<slug>.md` already exists. If so, ask whether to update or create a new variant.
8. Create `new-projects/<slug>.md` with this structure:

```
**Source:** <Person> (<context>, <date>)
**Status:** Needs outline

## What

<2-3 sentences: what this feature or system would do.>

## Why it matters

<2-3 sentences: why customers or the business care. Use specific signals if available (CS feedback, usage data, competitive pressure).>

## Open questions

<Bulleted list of things to resolve before this can be scoped or prioritized. Focus on unknowns that would change the approach.>
```

9. If the source includes enough detail, optionally add:

```
## Possible scope

<Bullet list of what might be in v1 vs. later. Keep rough; this isn't a PRD.>

## Related

<Links to existing vault docs, roadmap items, or external references.>
```

## Update the summary

10. Read `new-projects/README.md`.
11. Add a row to the summary table:

```
| [[<slug>]] | <Source person> | Needs outline | — |
```

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Use `[[wikilinks]]` for internal vault references
- Keep outlines short. This is a conversation starter, not a PRD.

## After filing

12. Report the file path.
13. If the idea came from a meeting or weekly log, suggest adding a reference link back to the source.
14. Ask: "Want me to add a task to the weekly log to review this with the team?"
