---
name: pdm:roadmap-summary
description: Summarize sections of the working roadmap by status and gaps
argument-hint: [section-name|"all"]
---

1. Read [[Q1 2026 roadmap working copy]] from the notes vault
2. Accept $ARGUMENTS as filter: section name (e.g., "App"), status (e.g., "active"), or "all"
3. For each project heading (###) in scope:
   - Name, staffing status emoji, unlock statement
   - Sub-items with dates → treat as completed
   - Count remaining open items
   - Flag: missing unlock statement, empty/TBD projects, dates in the past
4. Output: summary table (name, status, open items, flags) then detail for active projects
5. Skip detail for blocked/red projects unless requested
