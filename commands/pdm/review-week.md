---
name: pdm:review-week
description: Review this week's notes — priorities, gaps, stale items, and weekly metrics
argument-hint: [additional-instructions]
---

## Boards I work with

The only ClickUp folder I PM is **MTA**. Other folders in the workspace
(AI/Assistant, Platform Engineering, Integrations) belong to other PMs —
do **not** include them in "my boards" metrics, even when activity there is high.

**My folder:**
- `901314219314` — All Products → MTA. Lists: NB Data Health
  (`901326479735`), NB3.0 Iterations (`901321226863`), NB Onboarding
  (`901322053810`), Incrementality Beta (`901323301043`), R&D and future
  projects (`901324600443`), Alerts & Recommendations (`901324114765`),
  Product Analytics (`901326479726`).

Pass `folder_ids: ["901314219314"]` to `clickup_filter_tasks` for folder-level
queries. If MTA is quiet, report it as quiet — do not widen to other folders.

## Metrics collection

Collect these metrics **first**, before the qualitative review. All counts scoped
to the current week's log date range.

1. **TODOs completed** — count `- [x]` items in the weekly log file (including
   `#todone` tagged items). Also check `todos/done.md` for items with a `@date`
   inside this week's range.
2. **Tickets created (mine)** — call `clickup_resolve_assignees` for "me", then
   use `clickup_search` with `filters.creators: [<me>]` and
   `filters.created_date_from`/`to` to get tasks I created this week. Count them.
3. **Tickets completed (mine)** — use `clickup_filter_tasks` with
   `assignees: [<me>]`, `include_closed: true`, and `date_done_from`/`to` for the
   week. Count them.
4. **Tickets closed in MTA** — use `clickup_filter_tasks` with
   `folder_ids: ["901314219314"]`, `include_closed: true`, `date_done_from`/`to`
   for the week, and **no assignee filter**. This captures work shipped by
   Tong, Karim, Kamil, Kevin, and others in the lists I PM. Group the results
   by list (NB Data Health, NB3.0 Iterations, NB Onboarding, etc.) and report
   counts per list with a brief title list.
5. **Vault docs added** — run `git log --since=<monday> --until=<sunday>
   --name-status --diff-filter=A` and count new `.md` files under `discussions/`,
   `projects/`, `wiki/`, `product planning/`, and `release-notes/`. Exclude
   `log/`, `_templates/`, `gdrive/`, and `todos/`.
6. **PRDs written** — from the new-doc list above, count files whose name or
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
| ClickUp tickets created (mine) | N | brief list |
| ClickUp tickets completed (mine) | N | brief list |
| ClickUp tickets closed in MTA | N | per-list breakdown (e.g. NB3.0 Iterations: 4, NB Data Health: 2). Brief title list |
| Vault docs added | N | folder breakdown |
| PRDs written | N | names |

2. **What's most important** — top 3 priorities with reasoning
3. **What needs investigation** — items with insufficient detail or unresolved
   questions
4. **What needs more detail** — gaps, missing owners, undefined scope
5. **What should become tickets** — items with enough detail to act on but no
   ClickUp link. Include suggested ticket title and location.
