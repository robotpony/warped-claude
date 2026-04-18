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
   - **Done**: leave in previous log (marked `[x]`)
   - **Rolling forward**: open `- [ ]` items to carry into new log
   - **Dead**: items that appear stale (carried 2+ weeks with no progress noted). Flag these.
5. Clean up rolling items:
   - Group by workstream (diagnostics, search, benchmarks, NB 3.0, onboarding, notifications, admin, etc.)
   - Drop items that are now tracked in linked docs or ClickUp tickets (note the link instead)
6. Summarize what got done last week in 3-5 bullets.

## 3. Review priorities

7. Read the Development Priorities doc (`gdrive/Projects/Numerical List of Development Priorities CONFIDENTIAL.md`), top/current draft only (stop at first repeated section header).
8. For the App section, identify:
   - Staffed projects (🟢/🟡) with no tasks carrying forward — coverage gaps
   - Sub-items with dates in the next 2 weeks — upcoming deadlines
   - Items marked ← or ➕➕ — active focus areas

## 4. Propose next tasks

9. Based on carry-forward items + priority gaps + upcoming dates, propose a task list:
   - **P0**: blocking, time-sensitive, or foundational
   - **P1**: important but not urgent
   - **Focus**: items that need deep work blocks
   - **Parked**: items to track but not act on this week
10. Present the proposed list to the user for review before creating the file.

## 5. Create the weekly log

11. After user approval (or edits), create `log/2026/<Month D, YYYY>.md` using the template at `_templates/Week of @date.md`.
12. Populate:
   - **Goals and tasks this week**: approved task list, grouped by workstream
   - **Release notes**: pull tasks in "release notes" ClickUp status from NB3.0 Iterations (same as `/pdm:release-notes`)
   - **Context**: carry-forward summary including:
     - Key decisions and threads from previous week
     - Coverage gaps flagged from priorities review
     - Upcoming dates from priorities doc
     - Any OOO or scheduling notes
   - **Eng Log**: empty day sections (Friday through Monday)

## 6. Present

13. Show the user the complete new log.
14. Flag:
    - Items you had to guess at (priority, grouping, ownership)
    - Stale items you dropped (so user can override)
    - Priority coverage gaps (staffed projects with no tasks)

## Notes

- This command replaces running `/pdm:carry-forward` → `/weekly-log` → manual editing separately.
- Don't create the file until the user approves the proposed task list. The proposal step is the checkpoint.
- If the previous week's log has unprocessed meeting notes or transcripts in the vault, mention them but don't auto-summarize (use `/pdm:meeting-summary` for that).
- Canadian English spelling. No em-dashes in prose.
