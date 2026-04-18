---
name: pdm:clickup-report
description: Pull ticket creation/completion stats from ClickUp
argument-hint: ["this week"|"this month"|date-range]
---

1. Resolve "me" via clickup_resolve_assignees to get user ID
2. Accept $ARGUMENTS as time range: "this week" (default), "this month", or specific dates
3. Search tasks created by me in range (paginate via cursor if >50 results)
4. Search tasks completed by me in range (use date_done_from/to + include_closed)
5. Output:
   - **Created**: total count, grouped by list/folder
   - **Completed**: total count, grouped by list/folder
   - **Still open**: items I created that aren't done, grouped by status
