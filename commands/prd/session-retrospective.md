---
name: prd:session-retrospective
description: Review a working session and identify improvements to commands, skills, efficiency, and process
argument-hint: <optional: focus area or session notes path>
---

Review the current conversation session and produce a structured retrospective focused on tooling and process improvements.

## Analyze

1. Walk through the conversation history. Identify each distinct task or phase of work.
2. For each phase, note:
   - What was done
   - How many round trips it took (human ↔ assistant)
   - Where time was spent on mechanics vs. substance
   - Whether an existing skill or command could have handled it
   - Whether a new skill or command would prevent the manual work next time

3. Categorize findings into four buckets:

## Output structure

Write the retrospective directly in the conversation (not to a file) using this structure:

### What went well
Patterns that worked. Keep doing these.

### Skills and commands to improve or create
For each suggestion:
- **Name**: proposed skill/command name
- **Trigger**: when would this be used
- **What it replaces**: the manual steps it would automate
- **Effort to build**: small (single command file), medium (skill with references), large (multi-phase skill)

### Where the human can be more efficient
Honest, specific observations about how the user could front-load context, batch requests, or reduce round trips. Frame as suggestions, not criticism. Reference specific moments in the session.

### Efficiency wins already banked
Things from this session that won't need repeating (style decisions, new files, solved patterns).

## Rules

- Be specific. Reference actual moments in the session, not generic advice.
- Don't suggest skills for one-off tasks. Only suggest automation for patterns that will repeat.
- Don't pad the retrospective. If a session was efficient, say so.
- If $ARGUMENTS names a focus area, weight the analysis toward that area.
- Canadian English spelling. No em-dashes.
