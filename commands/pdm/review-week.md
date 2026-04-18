---
name: pdm:review-week
description: Review this week's notes — priorities, gaps, stale items, and weekly metrics
argument-hint: [additional-instructions]
---

## Metrics collection

Collect these metrics **first**, before the qualitative review. All counts scoped
to the current week's log date range.

1. **TODOs completed** — count `- [x]` items in the weekly log file (including
   `#todone` tagged items). Also check `todos/done.md` for items with a `@date`
   inside this week's range.
2. **Tickets created** — call `clickup_resolve_assignees` for "me", then use
   `clickup_filter_tasks` with the user's ID and the week's date range to get
   tasks created this week. Count them.
3. **Tickets completed** — use `clickup_filter_tasks` with `include_closed`,
   `date_done_from`/`date_done_to` for the week, filtered to the user. Count them.
4. **Vault docs added** — run `git log --since=<monday> --until=<sunday>
   --name-status --diff-filter=A` and count new `.md` files under `discussions/`,
   `projects/`, `wiki/`, `product planning/`, and `release-notes/`. Exclude
   `log/`, `_templates/`, `gdrive/`, and `todos/`.
5. **PRDs written** — from the new-doc list above, count files whose name or
   content contains "PRD" or "prd" (in `projects/` or `product planning/`).

## Qualitative review

6. Find the current week's log file in `log/2026/`
7. Read it fully
8. Read all `[[linked]]` documents referenced in the log
9. Identify:
   - Open TODOs by priority (P0/P1/P2, #focus tags)
   - Items with no owner, deadline, or detail
   - Stale items (referenced but no progress noted)
   - Context that's been captured in linked docs (redundant)
   - Threads that have no home (mentioned once, not linked)

## Output

Write the results to a summary file and display them in the conversation.

### Summary file

Create `log/2026/<Week Date> - summary.md` (matching the weekly log filename
with ` - summary` appended). The file starts with a wikilink back to the weekly
log and contains the sections below. No YAML frontmatter. No H1 title (Obsidian
renders the filename).

### Sections

1. **Metrics** table:

| Metric | Count | Notes |
|--------|-------|-------|
| TODOs completed | N | source breakdown |
| ClickUp tickets created | N | brief list |
| ClickUp tickets completed | N | brief list |
| Vault docs added | N | folder breakdown |
| PRDs written | N | names |

2. **What's most important** — top 3 priorities with reasoning
3. **What needs investigation** — items with insufficient detail or unresolved
   questions
4. **What needs more detail** — gaps, missing owners, undefined scope
5. **What should become tickets** — items with enough detail to act on but no
   ClickUp link. Include suggested ticket title and location.
