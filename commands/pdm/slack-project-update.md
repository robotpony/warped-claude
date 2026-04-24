---
name: pdm:slack-project-update
description: Generate a Slack-ready weekly project status update for stakeholders
argument-hint: <project-name> [--clickup] [--detailed] [additional instructions]
---

Produce a Slack-ready weekly status update for a single project area. Output is formatted for pasting directly into a Slack channel or thread.

## Parse arguments

1. Extract the project name from $ARGUMENTS. This is everything before the first `--` flag or the end of the string.
   - Examples: "diagnostics", "data health and onboarding", "search", "notifications"
2. Check for flags:
   - `--clickup`: also query ClickUp for related task activity
   - `--detailed`: produce a longer report with doc summaries instead of one-liners
3. Any remaining text after the project name and flags is additional context or instructions from the user. Incorporate it.

## Gather

4. Find the current weekly log in `log/2026/`. Read it fully.
5. Identify all content related to the project:
   - **Goals section**: match bold project headers (`**Name (Priority)**`) where the header contains the project name (case-insensitive, substring match). Extract all tasks and sub-tasks under matched headers.
   - **Eng Log section**: scan each day's entries for paragraphs, headers, or bullets that mention the project name or closely related terms.
   - **Context section**: extract carried-forward bullets that reference the project.
   - **Parked items**: note any parked items related to the project (relevant as "not this week" context).
   - **Release notes section**: extract entries related to the project.
6. Collect all `[[wikilinks]]` found within the matched content. Read each linked document that exists in `discussions/` or `projects/` (skip infographic PNGs, HTML files, and release-notes/). For each linked doc, extract the opening summary or first 2-3 paragraphs to understand what it covers.
7. Read the previous week's log. Extract only:
   - Context section bullets mentioning the project
   - Any items related to the project that were completed or carried forward (shows trajectory)
8. **ClickUp ticket activity (always):**
   - Identify the ClickUp list or board most relevant to the project. Use `clickup_get_list` to resolve by name if needed.
   - Use `clickup_filter_tasks` with `date_done_from`/`date_done_to` for the current week to find tickets completed this week.
   - Use `clickup_filter_tasks` with `statuses: ["release notes"]` to find tickets awaiting documentation.
   - Use `clickup_filter_tasks` with `statuses: ["in progress", "in review"]` to see active work.
   - Group completed tickets by theme (bug fixes, new features, UX polish, cleanup) rather than listing each individually.
9. If `--clickup` flag is set, also do broader searches:
   - Use `clickup_search` with the project name as keyword to find related tasks across other lists
   - Check for tasks created this week that signal new scope

## Synthesize

9. From the gathered content, build four lists:

**Progress** (decisions made, docs written, research completed, tasks checked off, features shipped):
- Pull from `[x]` items in the Goals section
- Pull from Eng Log entries (research summaries, docs created, infographics produced)
- Pull from linked docs created or substantially updated this week
- Include ClickUp tickets completed this week, grouped by theme

**In progress** (active work, research underway, things with momentum):
- Pull from open `[ ]` items in the Goals section that show partial progress or are P0/P1
- Pull from Eng Log entries describing ongoing work
- Include ClickUp tickets in active/in-review statuses

**Coming next** (planned work, sequenced items, things that depend on this week's output):
- Infer from open tasks and their dependencies
- Pull from Context section threads that name next steps
- Pull from linked docs that outline future phases or open work

**Blockers and open questions** (things stuck on other people, decisions needed, risks):
- Pull from items tagged with blocking language or waiting-on-someone patterns
- Pull from Context section risk callouts
- Pull from the Parked items section if the project has parked work with stated reasons

10. For each list item, prefer outcome language over task language:
    - Good: "Validated three-witness audit model for page view reconciliation"
    - Bad: "Reviewed onboarding diagnostic health doc"
    - Good: "Waiting on Tyler for 15-min overlap conversation; risk of duplicate Retool work"
    - Bad: "Need to sync with Tyler"

## Format

11. Output the status update in this structure:

```
:fyi: <Project Name> — Week of <date> :fyi:

Progress: <optional parenthetical cross-reference, e.g. "(see #product-releases for release notes)">
<ticket count as context line, e.g. "12 tickets completed this week on the NB3.0 Iterations board">
<2-5 bullets, outcome-focused, grouped by theme>

In progress (<ticket count>):
<1-4 bullets, what's actively being worked on and by whom>

<Optional: "Release notes pending (<count>):" if tickets are awaiting documentation>

Next:
<1-3 bullets, what's coming and any sequencing>

Blockers:
<0-3 bullets, only if stakeholder-visible>
```

12. **Section naming**: use "Progress" (not "Done") as the first section header. It better fits weeks with a mix of shipped features, bug fixes, and incremental work.

13. **Ticket counts as context**: lead sections with ticket counts where available (e.g., "12 tickets completed", "In progress (10 tickets)"). Gives scale without listing every ticket.

14. **Cross-references over duplication**: if release notes or detailed content exists in another Slack channel (e.g., #product-releases), reference it with a parenthetical rather than repeating the content.

15. **Links are optional**: only include URLs when they genuinely help the reader (e.g., linking a board someone might not know about). For well-known boards and channels, names are sufficient. Don't link for the sake of linking.

16. **Blockers section is optional.** Only include if there are blockers visible and actionable by the audience. Internal task-level blockers belong in the saved doc, not the Slack post.

17. If `--detailed` was passed, expand each bullet with 1-2 sentences of context and include a "Key references:" list of linked URLs at the bottom.

## Save and link

15. Save the update to `discussions/<project-slug>-update-<month>-<day>-<year>.md` (e.g., `discussions/diagnostics-update-april-24-2026.md`). Use the project name slugified as the prefix. The saved doc version MAY include `[[wikilinks]]`, blockers, and internal detail that's useful for vault reference but not appropriate for Slack.
16. Link the saved doc from the current weekly log under the appropriate day in the Eng Log section. Use the format:
    - `- Project update: [[<filename>]]`
    - Place it under the current day's header (e.g., `## Friday`). If no header exists for today, create one.
17. Also output the Slack-ready version to the conversation so the user can copy it directly. This version uses Slack formatting (no markdown headers, no wikilinks, real URLs inline).

## Rules

- **Length**: keep the Slack output under 300 words, 12 bullets max. This should fit in a single Slack message without scrolling. Err on the side of fewer, tighter bullets over comprehensive coverage.
- **Tone**: direct, specific, product-writing-rules style. No corporate filler ("making progress on," "continuing to iterate"). State what happened and what it means.
- **Attribution**: name people when work is blocked on them or when someone else did notable work. Don't attribute every line to Bruce.
- **Decisions over activity**: "Decided to scope diagnostics to web-only for Shopify" is more useful than "Had a meeting about diagnostics scope."
- **Links are earned**: only include URLs when they help the reader find something they wouldn't otherwise know about. Well-known boards and channels can be referenced by name. Don't link for the sake of linking.
- **No markdown headers in Slack output**: Slack doesn't render `##`. Use `:fyi:` emoji bookends for the title. Section headers are plain bold text (`Progress:`, `In progress:`, `Next:`).
- **Cross-reference, don't duplicate**: if release notes or detailed content exists in another Slack channel, reference it with a parenthetical (e.g., "(see #product-releases for release notes)") rather than repeating the content.
- **Don't include**: parked items (unless they were actively parked this week as a decision), admin tasks, meeting logistics, internal-only analysis docs without shareable URLs.
- **Two versions**: the saved vault doc can include wikilinks, blockers, key docs lists, and internal detail. The Slack output is leaner: fewer bullets, real URLs, no wikilinks, blockers only if stakeholder-relevant.
- No em-dashes in output (use commas, semicolons, or separate sentences).
- Canadian English spelling (colour, behaviour, organize, optimize).
