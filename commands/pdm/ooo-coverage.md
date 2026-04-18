---
name: pdm:ooo-coverage
description: Generate an OOO coverage doc from active projects and weekly logs
argument-hint: <dates, e.g. "April 9-10, 13-14">
---

Build an out-of-office coverage document for the given dates.

## Gather context

1. Accept $ARGUMENTS as the OOO date range.
2. Read the current week's log in `log/2026/`.
3. Read all `[[linked]]` project docs referenced in the log.
4. Read any files in `projects/` that appear active (recently modified, referenced in log).
5. Scan open `- [ ]` TODOs across log and project files for anything time-sensitive.

## Build the document

Create `projects/Out of office [dates].md` with this structure and order:

### 1. TL;DR

Three bullet points:

- **Out**: the OOO dates
- **Back online**: the next working day after the OOO range
- **Availability**: Default to "Limited Slack, no approvals. Reach out to contacts below." Don't overstate unavailability (never say "offline" or "no Slack" unless the user specifies it).

### 2. Coverage

Simple table. One row per project that may need attention while out.

| Project | Contact | Focus while I'm out |
|---|---|---|

Rules:
- **Contact**: the specific person to ping. Use — if no one is carrying it.
- **Focus**: one sentence on what might come up or need doing. Keep it actionable.
- Only include projects where something could realistically need attention during the OOO window. Projects with no action needed get omitted from this table.

### 3. Next steps by project

One H3 subsection per active project, ordered by priority (P0 first). Include priority label in the heading, e.g. `### Diagnostics (P0)`.

Each subsection is a flat bullet list of concrete next steps. Rules:
- Don't duplicate items already covered by a contact assignment in the Coverage table
- Keep items actionable: who needs to do what, or what decision is needed
- Name the person responsible where known
- Reference triaged tickets and linked docs with `[[wikilinks]]` where they exist
- For projects with nothing open, use a single line: "No action needed while I'm out." or "Largely complete. No action needed."
- For projects on hold, state what happens when you return

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Keep the whole doc under 2 pages when rendered
- No section should be empty; if nothing fits, omit the section

## Present and export

1. Show the draft to the user. Flag anything you had to guess at or couldn't find source material for.
2. Ask if the user needs a Word/RTF export. If yes, convert to .docx using pandoc after the draft is approved.
