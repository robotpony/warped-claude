---
name: pdm:priorities-sync
description: Snapshot current roadmap priorities status from the Development Priorities doc
argument-hint: [section-name, e.g. "App" or "all"]
---

Read the Development Priorities doc and produce a current status snapshot.

## Input

1. Accept $ARGUMENTS as a section filter. Default to "App" (the user's area). Use "all" for full doc.
2. Fetch the Notion Development Priorities **data source directly** with `mcp__claude_ai_Notion__notion-fetch` on `collection://020011e4-1c05-42f7-8c5d-7ec61bd2adad`. This returns the schema plus all page entries in one call. Do not fetch the database wrapper URL first — the data source ID is stable. Do not read the deprecated `gdrive/Projects/Numerical List of Development Priorities CONFIDENTIAL.md`.
3. Filter the returned entries client-side by the `Area` property (`App`, `Incrementality`, `Data Out`, `Data In`, `Pipeline Core`, `AI`, `Everything Else`). **Do not use `notion-search` to enumerate a section** — search returns semantic matches across all areas and forces per-page verification fetches. Search is for keyword discovery, not section listing.

## Extract

4. For the requested section(s), extract each project:
   - Priority number (1, 2, 3...)
   - Staffing status (🟢 fully staffed, 🟡 partially staffed, 🔴 not staffed)
   - Project title
   - Strategic marker (⭐) if present
   - New marker (🆕) if present
   - Sub-items with dates (completed or upcoming)
   - Sub-items with ← markers (active focus)
   - Sub-items with ➕➕ markers (recently added)

5. Cross-reference with the current weekly log in `log/2026/`:
   - Which priorities have active tasks in the log?
   - Which priorities have no representation in the log?

## Output

6. Present a status table:

```
## Priorities Snapshot — <section> (<date>)

| # | Status | Project | Active in Log? | Notes |
|---|--------|---------|---------------|-------|
```

7. Below the table, add:

**Active this week:** Projects with tasks in the current weekly log.

**No coverage:** Staffed projects (🟢/🟡) with no tasks in the log. Flag these — if they're staffed, someone should be working on them.

**Upcoming dates:** Any sub-items with future dates in the next 2 weeks.

**Changes since last sync:** If a previous priorities sync exists in `discussions/` or the log, note what moved (new projects, staffing changes, completed items). If no previous sync exists, skip this section.

## Optionally update the weekly log

8. Ask: "Want me to add coverage gaps or upcoming dates to the weekly log?"
   - If yes, add flagged items to the appropriate section with `#priorities-sync` tag.

## Notes

- The Notion priorities database is the source of truth per CLAUDE.md. Don't editorialize on priority order.
- Dates in the doc may be past — note whether they appear completed or overdue.
- Staffing info at the top of each section is important context. Include it in the snapshot header.
