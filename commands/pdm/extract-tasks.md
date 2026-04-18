---
name: pdm:extract-tasks
description: Extract action items from a vault note, attribute owners, and offer to insert into the weekly log
argument-hint: <path-to-note> [--for me|--all]
---

Scan a vault note for action items and organize them for tracking.

## Input

1. Accept $ARGUMENTS as a file path (or [[wikilink]] name). If no path given, ask the user.
2. Read the file fully.
3. Determine extraction scope:
   - `--for me` or no flag: extract only tasks owned by or relevant to the vault owner (Bruce)
   - `--all`: extract all tasks regardless of owner

## Extract

4. Identify action items from:
   - Explicit assignments ("Bruce will...", "**Bruce**: do X")
   - Existing `- [ ]` checkboxes
   - Implied tasks with no owner that fall in the user's area of responsibility (App team projects per CLAUDE.md)
   - Decisions that require follow-up ("we decided X" → someone needs to implement X)
   - Open questions that need resolution ("TBD", "figure out", "needs a decision")

5. For each task, capture:
   - **Owner**: who is responsible (use "unowned" if unclear)
   - **Task**: what needs to be done, in one line
   - **Source context**: which topic or section it came from
   - **Priority**: infer from language (urgent/blocking = P0, should/next = P1, could/eventually = P2)
   - **Roadmap link**: map to a Development Priorities project number if obvious

## Present

6. Show the extracted tasks grouped by owner:

```
### Your tasks (Bruce)
- [ ] <task> — <source context> #<priority>

### Other people's tasks
- [ ] **<Owner>**: <task> — <source context>

### Unowned / implied
- [ ] <task> — <source context> (needs an owner)
```

7. Ask: "Add your tasks to the weekly log? Which section?"

## Insert into weekly log

8. If yes, find the current weekly log in `log/2026/`.
9. Insert tasks into the section the user specifies, or suggest a section based on the task's workstream.
10. Tag inserted tasks with the source note name for traceability (e.g., `#design-review` if extracted from a design review note).
11. Don't duplicate tasks already in the log (match by description similarity).

## Notes

- Be conservative about what counts as a task. Observations, opinions, and context are not tasks.
- "We should think about X" is not a task. "Define X and write a brief" is.
- When in doubt, include it as "unowned / implied" and let the user decide.
