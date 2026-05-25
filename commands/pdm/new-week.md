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

3. **Spawn a `general-purpose` sub-agent to read the heavy files.** Do NOT read the previous week's log or its linked docs in main context — they're large and the main context needs to stay light for the proposal step, the Notion fetch, and the final write. Use the contract below verbatim; the sub-agent's structured response is what you'll work from.

   <agent-contract>
   You are reading the previous week's engineering log and its linked vault docs so the main agent can do a weekly carry-forward without loading the full content. The vault is at `/Users/bruce/notes`. The previous week's log is at `log/2026/<previous Monday's date>.md` (the main agent will tell you the exact path).

   Do this:

   1. Read the previous week's log in full.
   2. For every `[[wikilink]]` in that log, resolve it to a vault file path and read that file in full too. Include the week's summary file at `log/2026/summaries/<date> - summary.md` if it exists.
   3. Also read the week-before's log (one week earlier) so you can detect items carried 2+ weeks.
   4. Return your findings in EXACTLY the shape below — no prose intro, no editorializing. Open-item lines must be verbatim copies (preserve every tag, link, italic note, and trailing marker). The main agent re-emits them into the new log; paraphrasing breaks that.

   ## Open items (verbatim)
   Group by the workstream heading they appear under in the log. For each item, give the verbatim line including all priority tags, dates, italics, and links.

   ### <workstream heading>
   - <verbatim `- [ ]` line>
   - ...

   ## Completed last week
   - Count: N items
   - Buckets: <e.g., "8 Notion sunsets, 6 hub cleanups, 6 publishes, 6 admin">

   ## Stale (carried 2+ weeks)
   For each item that appears in BOTH the previous week's log and the week-before's log as `- [ ]`:
   - "<item subject>" — first appeared <date>, carried <N> weeks, committed <date if any>

   ## Dropped candidates
   Items that look like throwaway notes (no owner, no date, captured as bullets without checkboxes, or one-line provocations like "POD chat" entries). List them so the main agent can confirm.

   ## What got done (3-5 bullets)
   Synthesize the week's outcomes. Lead with the most important. Don't list every checked item — bucket them.

   ## Open threads with no owner
   Items mentioned in the log or linked docs that surface questions/ideas without a clear next step.

   ## Linked docs read
   - <path>
   - ...

   ## Meeting notes detected
   List any `discussions/*.md` files referenced in the week's log so the main agent can mention them in Context.

   That's the entire response. No introduction, no closing summary, no "let me know if you need more." The main agent will parse this directly.
   </agent-contract>

4. **Tag carried items in the previous week's log with `#moved`.** Do this in ONE shot, not per-line — a per-line loop races vault linters and bloats context. Use a single `Bash` `sed` invocation, e.g.:
   ```bash
   sed -i '' '/^- \[ \]/ s/$/ #moved/' "log/2026/<previous Monday>.md"
   ```
   This tags every open `- [ ]` line. If the previous log has open items you've decided to *drop* (not carry), edit those lines separately after the bulk tag. **Skip this step if it errors twice** — it's an audit-trail nicety, not load-bearing.

5. From the sub-agent's structured response:
   - **Rolling forward** = "Open items" section, minus anything in "Dropped candidates" or anything the user explicitly kills during proposal review.
   - **Done** stays in the previous log; do NOT carry. The new log's "Goals and tasks this week" starts with zero `[x]` items.
   - **Stale** items get flagged in-line in the proposal (e.g., `*(carried 2w — getting stale)*`) so the user can drop or recommit.
6. Group rolling items by workstream. Drop items now tracked in linked docs or ClickUp tickets (note the link instead).
7. **Verify before proceeding:** the rolling-forward set contains only `- [ ]` items. If any `- [x]` slipped in, remove it.
8. The "What got done" bullets from the sub-agent become the carry-forward summary in step 17.

## 3. Review priorities

9. Fetch the Notion Development Priorities **data source directly** with `mcp__claude_ai_Notion__notion-fetch` on `collection://020011e4-1c05-42f7-8c5d-7ec61bd2adad` (one call returns schema + all entries). Filter client-side by `Area: "App"`. Do not use `notion-search` to enumerate — it returns semantic matches across all areas. Do not read the deprecated `gdrive/Projects/Numerical List of Development Priorities CONFIDENTIAL.md`.
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
    - **Parked changes**: items to add to, remove from, or update in `todos/parked.md` (the parked items log). Do NOT propose a "Parked" section in the new weekly log — parked items live in `todos/parked.md`, not the weekly log.
15. Present the proposed list to the user for review before creating the file. **Every item in the proposal must be `- [ ]` (open). If a `- [x]` item appears in the proposal, treat that as a bug and remove it before showing the user.** Also surface proposed edits to `todos/parked.md` (additions, removals, or rewording) for approval in the same step.

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
18. **Apply approved edits to `todos/parked.md`** (additions, removals, rewording). Do not add a "Parked items" section to the new weekly log.
19. **Final check before saving:** grep the new log for `- [x]` in the "Goals and tasks this week" section. The count must be zero. If any are present, remove them. Also confirm the new log has no `# Parked items` heading.

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
