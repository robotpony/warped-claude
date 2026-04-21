---
name: pdm:reminders
description: Review weekly log for recurring tasks, top priorities, and missed rituals
argument-hint: [week-date or "update" to edit reminders list]
---

Surfaces recurring tasks and rituals from `wiki/weekly-reminders.md`, checks them against recent weekly logs, and suggests new patterns.

## 1. Determine mode

If $ARGUMENTS contains "update" or "edit", skip to step 6 (maintenance mode).

Otherwise, proceed with the review.

## 2. Load reminders and logs

1. Read `wiki/weekly-reminders.md` fully.
2. Determine the target week. Use $ARGUMENTS as the date if provided, otherwise use `currentDate` to find the current week.
3. Read the current week's log in `log/2026/`.
4. Read the previous 4-5 weekly logs (enough to detect patterns and missed items).

## 3. Check due reminders

For each reminder in `wiki/weekly-reminders.md`:

- **Weekly** (`#recurring-weekly`): always due.
- **Biweekly** (`#recurring-biweekly`): due on even-numbered weeks of the year. Check whether the item appeared in the previous week's log; if it did, skip this week.
- **Monthly** (`#recurring-monthly`): due if no evidence of completion in the last 4 weekly logs.
- **Quarterly** (`#recurring-quarterly`): due if within 2 weeks of the noted date, or if no evidence of completion in the last 12 weeks.

For each due reminder, check whether a matching task already exists in the current week's log (fuzzy match on the task description, not exact string). Mark as:
- **Covered**: a matching task exists in the current log
- **Missing**: no matching task found; needs to be added
- **Done**: a matching `[x]` item exists in the current log

## 4. Scan for missed reminders from last week

Read the previous week's log. For each reminder that was due last week, check:
- Was it completed (`[x]` or `#todone`)?
- Was it carried forward (`#moved`)?
- Was it absent entirely?

Report missed items (absent or carried without completion) separately, so the user can decide whether to add them this week or let them go.

## 5. Discover new patterns

Scan the last 4-6 weekly logs for tasks that recur 3+ times with similar wording. Compare against the existing reminders list. Suggest any recurring tasks not already tracked, with:
- The approximate wording that appeared
- How many weeks it appeared
- Suggested cadence (weekly, biweekly, monthly)
- Suggested `@owner` based on attribution in the logs (default `@me`)

Present suggestions; don't auto-add them.

## 6. Maintenance mode (if "update" or "edit" in $ARGUMENTS)

Read `wiki/weekly-reminders.md` and present the current list. Ask the user what to add, remove, or change. Update the file after confirmation.

When adding new reminders:
- Default owner to `@me` unless specified
- Require a cadence tag (`#recurring-weekly`, `#recurring-biweekly`, `#recurring-monthly`, `#recurring-quarterly`)
- Use checkbox format (`- [ ]`) to match vault conventions

## Output

Present results grouped into four sections:

**Due this week** — reminders that should appear in this week's log, with status (covered / missing / done).

**Missed last week** — reminders that were due but not completed or carried forward. Brief, not guilt-inducing; just factual.

**Suggested new reminders** — patterns discovered from log scanning. Include the evidence (which weeks, approximate wording).

**Current reminders list** — the full contents of `wiki/weekly-reminders.md` for reference, so the user can spot anything to update.

## Notes

- Attribution: all reminders default to `@me` (Bruce). Other owners can be specified with `@name`.
- This command does not modify the weekly log. It reports; the user (or `/pdm:new-week`) acts on it.
- When suggesting new patterns, err toward suggesting rather than staying silent. The user can dismiss suggestions easily; missing a pattern is harder to recover from.
- Fuzzy matching: "Provide Benno with updated priority list" and "Get benno a summary of roadmap" are the same recurring task. Match on intent, not exact wording.
- Canadian English spelling. No em-dashes in output.
