---
name: pdm:meeting-summary
description: Summarize a meeting transcript or raw notes into a structured vault note
argument-hint: <path-to-transcript-or-notes-file>
---

Produce a structured meeting summary from a transcript or raw notes file.

## Input

1. Accept $ARGUMENTS as the path to a transcript or notes file. If no path given, ask the user.
2. Read the file. If it exceeds read limits, read in chunks — do not skip content.
3. Identify the meeting name, date, and attendees from the content. Ask the user to confirm or fill gaps.

## Analyze

4. Identify distinct topics discussed. For each topic:
   - Assign a short title
   - Note the presenter/driver if identifiable
   - Map to a roadmap priority if one exists (fetch the data source directly via `mcp__claude_ai_Notion__notion-fetch` on `collection://020011e4-1c05-42f7-8c5d-7ec61bd2adad` — one call returns all entries; do not read the deprecated gdrive copy)
   - Note approximate time range if timestamps are present

5. For each topic, extract:
   - **Decisions made** — who decided, what was decided, any conditions
   - **Tasks assigned** — who owns it, what specifically they need to do
   - **Key points by speaker** — attribute substantive contributions to the person who made them. Skip pleasantries and filler.
   - **Open questions** — things raised but not resolved

6. Assess overall sentiment and implied work:
   - Energy/alignment level per topic
   - Implied tasks not explicitly assigned (unowned work, shelved concerns, accepted trade-offs)
   - Cross-cutting themes that span multiple topics

## Write the summary

7. Create `discussions/<Meeting name> <date>.md` with this structure:

```
## <Meeting name> — <date>

**<Meeting type>** | <duration> | <platform>
**Attendees:** <list>
**Source:** <link if available>

---

### Topics and Themes

| Topic | Presenter | Roadmap Link | Time |
|-------|-----------|-------------|------|

<one-line summary of dominant theme>

---

### Decisions and Tasks by Topic

#### <Topic 1>

**Decisions:**
- <decision with attribution>

**Tasks:**
- [ ] **<Owner>**: <task description>

<repeat for each topic>

---

### Detailed Notes by Topic

#### <Topic 1> (<Presenter>)

<Attributed notes, organized by speaker contribution>

<repeat for each topic>

---

### Sentiment and Implied Tasks/Themes

**Sentiment:**
- <per-topic energy/alignment observations>

**Implied tasks not explicitly assigned:**
- [ ] <task with context on why it matters>

**Cross-cutting themes:**
- <theme with explanation>
```

## Link into the weekly log

8. Find the current weekly log in `log/2026/`.
9. Add a reference in the Context section: `- [[<filename>]] — <one-line summary of key decisions>`
10. Ask the user: "Want me to pull your tasks from this summary into the weekly log?"
    - If yes, extract tasks owned by or relevant to the user, tag with `#meeting-notes #todo`, and insert into the appropriate section of the log.

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Attribute quotes and positions to specific people
- Use `[[wikilinks]]` for internal vault references
- Keep decisions and tasks scannable — one line per item
- Don't editorialize; capture what was said, not what should have been said
