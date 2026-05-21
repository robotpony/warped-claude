---
name: mx:weekly-changelog
description: Summarize the current project's changelog for a given week, formatted for the weekly log
argument-hint: [week-of YYYY-MM-DD | last]
---

# Summarize this week's changelog: $ARGUMENTS.

Produce a short, paste-ready summary of `CHANGELOG.md` entries from the current project that fall within a given week, formatted for inclusion in the weekly log.

Follow these steps:

1. **Find the changelog.** Look for `CHANGELOG.md` at the project root. If absent, check `CHANGES.md`, `HISTORY.md`, or fail with a clear "no changelog found in <cwd>" message.

2. **Resolve the date window.**
   - Default: the current ISO week (Monday 00:00 → now). Use `date` to determine today's day-of-week and back up to Monday.
   - `$ARGUMENTS` of `last` → the previous ISO week (Monday → Sunday).
   - `$ARGUMENTS` matching `YYYY-MM-DD` → the ISO week containing that date.
   - State the resolved window in a one-line preamble so the user can sanity-check.

3. **Parse changelog entries** that fall within the window. Entries look like `## [0.12.2] — 2026-05-15`; the date after the em-dash is what matters. Skip entries outside the window. If zero entries match, say so and stop.

4. **Synthesize the summary.** Goal: weekly-log-ready markdown the user can paste under their project section. Prefer this shape:

   ```
   ### <project name> (week of YYYY-MM-DD)

   <one-or-two-sentence overview: range of versions shipped, headline themes>

   - **<headline> (vX.Y.Z)**: <one short sentence — the substantive change, not the file list>
   - **<headline> (vX.Y.Z)**: ...
   ```

   - Project name comes from the working directory's name or `pyproject.toml` if present.
   - One bullet per version. Lead with what the user would care about a week from now (the user-visible change), not the implementation details.
   - Group consecutive small patches under one bullet if they share a theme (e.g., three patch-level polish bumps → one "polish pass on X" bullet) — but keep version numbers visible.
   - Drop bullets whose content is purely internal (refactors, dep bumps) unless the changelog framed them as significant.
   - Match the project writing rules: direct, conclusion-first, no corporate jargon, no em-dashes in prose (em-dashes in version headers are fine since they're verbatim).

5. **Output to stdout.** Don't modify any files; the user will paste into their weekly log themselves. Print the resolved date window line above the summary so it's visible in the chat too.

6. **Don't make commits.** This command is read-only on the project.

## Examples

Default (current week):

```
/mx:weekly-changelog
```

Previous week:

```
/mx:weekly-changelog last
```

Specific week containing a date:

```
/mx:weekly-changelog 2026-05-08
```
