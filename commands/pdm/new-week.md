---
name: pdm:new-week
description: Monday ritual — carry forward, review priorities, propose tasks, and create the weekly log
argument-hint: [date, e.g. "April 13, 2026"]
---

Combined weekly kickoff. Runs carry-forward, reviews priorities, proposes next tasks, and creates the new weekly log in one pass.

## 1. Determine dates

1. Accept $ARGUMENTS as the new week's date. If not provided, use the Monday of the current week from `currentDate`.
2. Find the previous week's log in `log/2026/`.

## 2. Carry forward

3. Read the previous week's log fully, including all `[[linked]]` documents.
4. Separate TODOs into:
   - **Done**: completed items (`- [x]`). **Leave these in the previous log. Do NOT carry them into the new log under any circumstance, including items completed but tagged `*(carried)*`, `#todone`, or with a `@date`. The "Goals and tasks this week" section in the new log starts with zero `[x]` items.**
   - **Rolling forward**: open `- [ ]` items only. These are the items to carry into the new log.
   - **Dead**: items that appear stale (carried 2+ weeks with no progress noted). Flag these for user decision; do not carry them silently.
5. In the **previous** week's log, tag every carried-forward item with `#moved` so it's
   clear the item migrated. (Add `#moved` at the end of the line; don't change the
   checkbox state.)
6. Clean up rolling items:
   - Group by workstream (diagnostics, search, benchmarks, NB 3.0, onboarding, notifications, admin, etc.)
   - Drop items that are now tracked in linked docs or ClickUp tickets (note the link instead)
7. **Verify before proceeding:** the rolling-forward set contains only `- [ ]` items. If any `- [x]` item is in the set, remove it.
8. Summarize what got done last week in 3-5 bullets.

## 3. Review priorities

9. Read the Development Priorities doc (`gdrive/Projects/Numerical List of Development Priorities CONFIDENTIAL.md`), top/current draft only (stop at first repeated section header).
10. For the App section, identify:
    - Staffed projects (🟢/🟡) with no tasks carrying forward — coverage gaps
    - Sub-items with dates in the next 2 weeks — upcoming deadlines
    - Items marked ← or ➕➕ — active focus areas

## 4. Surface reminders

11. Read `wiki/weekly-reminders.md`.
12. For each due reminder (weekly items are always due; biweekly/monthly/quarterly per the cadence rules in `/pdm:reminders`):
    - Check whether a matching task already exists in the carry-forward items. If so, skip it (already covered).
    - Otherwise, add it to a **Reminders** group in the proposed task list, tagged `#reminder`.
13. Check the previous week's log for reminders that were due but never completed or carried. Flag these as "missed last week" in the proposal so the user can decide.

## 5. Propose next tasks

14. Based on carry-forward items + priority gaps + upcoming dates + due reminders, propose a task list:
    - **P0**: blocking, time-sensitive, or foundational
    - **P1**: important but not urgent
    - **Focus**: items that need deep work blocks
    - **Reminders**: due recurring tasks from `wiki/weekly-reminders.md`, tagged `#reminder`
    - **Parked**: items to track but not act on this week
15. Present the proposed list to the user for review before creating the file. **Every item in the proposal must be `- [ ]` (open). If a `- [x]` item appears in the proposal, treat that as a bug and remove it before showing the user.**

## 6. Create the weekly log

16. After user approval (or edits), create `log/2026/<Month D, YYYY>.md` using the template at `_templates/Week of @date.md`.
17. Populate:
    - **Goals and tasks this week**: approved task list, grouped by workstream. **All items start as `- [ ]` open.** Reminders go in an **Admin / Rituals** workstream group (not scattered across project groups).
    - **Release notes**: pull tasks in "release notes" ClickUp status from NB3.0 Iterations (same as `/pdm:release-notes`)
    - **Context**: carry-forward summary including:
      - Key decisions and threads from previous week
      - Coverage gaps flagged from priorities review
      - Upcoming dates from priorities doc
      - Any OOO or scheduling notes
    - **Eng Log**: empty day sections (Friday through Monday)
18. **Final check before saving:** grep the new log for `- [x]` in the "Goals and tasks this week" section. The count must be zero. If any are present, remove them.

## 7. Present

19. Show the user the complete new log.
20. Flag:
    - Items you had to guess at (priority, grouping, ownership)
    - Stale items you dropped (so user can override)
    - Priority coverage gaps (staffed projects with no tasks)
    - Confirm in the message: "Goals and tasks this week starts with N open items, zero completed."

## Notes

- This command replaces running `/pdm:carry-forward` → `/weekly-log` → manual editing separately.
- Don't create the file until the user approves the proposed task list. The proposal step is the checkpoint.
- If the previous week's log has unprocessed meeting notes or transcripts in the vault, mention them but don't auto-summarize (use `/pdm:meeting-summary` for that).
- Canadian English spelling. No em-dashes in prose.
