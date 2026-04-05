---
name: prd:transcript-review
description: Review a meeting transcript and produce a structured design/product review summary
argument-hint: <path-to-transcript>
---

Review a meeting transcript and produce a structured summary optimized for product and design decision-making. Different from `pdm:meeting-summary` (which targets vault notes and weekly logs) — this produces a standalone review document alongside the source transcript.

## Input

1. Accept $ARGUMENTS as the path to a transcript file. If no path given, ask the user.
2. Read the file. If it exceeds read limits, read in chunks. Do not skip content.
3. Identify the meeting name, date, participants (with roles/companies), and platform from the content.

## Analyze

4. Identify the primary artifact or feature being reviewed. Write a concise overview:
   - What was presented
   - What problem it solves
   - Who presented it
   - How far along it is (concept, prototype, development preview, production)

5. Extract all substantive questions and answers:
   - Format as **Q:** / **A:** pairs
   - Attribute the asker and answerer
   - Include the actual answer, not just "they discussed it"
   - If a question went unanswered, note it as open

6. Extract all decisions:
   - What was decided
   - Who made or confirmed the decision
   - Any conditions or caveats attached
   - Use direct quotes where the decision moment is clear

7. Extract all actions:
   - Format as a table: Owner | Action
   - Only include actions that were explicitly assigned or volunteered
   - Don't infer actions that weren't stated

8. Collect open/unresolved items:
   - Questions that weren't answered
   - Debates that ended without resolution
   - Dependencies or blockers identified but not solved
   - Future work mentioned but not scoped

## Write the summary

9. Create the summary file beside the source transcript, named `{source-name-without-extension}-summary.md`.

```
# <Feature/Topic>: Design Review Summary

**Meeting:** <name>, <date>
**Presenter:** <name>
**Attendees:** <list with roles>

---

## Overview

<3-5 paragraph overview of what was presented and why>

---

## Questions & Answers

**Q: <question>** (<asker> to <answerer>)
A: <answer>

<repeat>

---

## Decisions

- **<decision>** <context and attribution>

---

## Actions

| Owner | Action |
|-------|--------|

---

## Open / Unresolved

- **<item>** — <context>
```

10. Present the summary path and a one-line description to the user.

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Attribute positions to specific people with direct quotes where useful
- Don't editorialize or add recommendations. Capture what was said.
- Bold key phrases in answers for scannability, not full sentences
