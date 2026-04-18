---
name: pdm:standup-prep
description: Generate a standup summary from the weekly log and recent ClickUp activity
argument-hint: [additional-context]
---

Produce a concise standup update: what got done, what's in progress, what's blocked.

## Gather

1. Find the current weekly log in `log/2026/`. Read it fully.
2. Check for any items marked `#todone` with today's or yesterday's date.
3. Read the Eng Log entries for the most recent 1-2 days that have content.
4. Search ClickUp for tasks assigned to the user (Bruce, user ID 106188160) that were updated in the last 2 days:
   - Use `clickup_filter_tasks` with assignee filter and `order_by: "updated"`.
   - Note status changes (moved to done, in progress, release notes, etc.).
5. If $ARGUMENTS contains additional context, incorporate it.

## Format

6. Produce a standup in this format:

```
### Standup — <date>

**Done:**
- <completed items from log + ClickUp, 2-4 bullets>

**Today:**
- <open P0/focus items from log, 2-3 bullets>

**Blocked/Waiting:**
- <items waiting on someone else, with who and what>
- <if nothing blocked, say "Nothing blocked.">
```

## Rules

- Keep it to 3-8 bullets total. This is for a quick verbal update, not a status report.
- Lead each bullet with the outcome or action, not the project name.
- Name people when something is blocked on them.
- Don't include parked or low-priority items.
- If there's an OOO coming up, add a one-line note at the bottom.
- Don't write to any file — just output to the conversation. The user will copy what they need.
