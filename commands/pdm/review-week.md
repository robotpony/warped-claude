---
name: pdm:review-week
description: Review this week's notes — priorities, gaps, and stale items
argument-hint: [additional-instructions]
---

1. Find the current week's log file in log/2026/
2. Read it fully
3. Read all [[linked]] documents referenced in the log
4. Identify:
   - Open TODOs by priority (P0/P1/P2, #focus tags)
   - Items with no owner, deadline, or detail
   - Stale items (referenced but no progress noted)
   - Context that's been captured in linked docs (redundant)
   - Threads that have no home (mentioned once, not linked)
5. Output structured as:
   - **What's most important** — top 3 priorities with reasoning
   - **What needs investigation** — items with insufficient detail or unresolved questions
   - **What needs more detail** — gaps, missing owners, undefined scope
   - **What should become tickets** — items with enough detail to act on but no
     ClickUp link. Include suggested ticket title and list/folder if obvious.
