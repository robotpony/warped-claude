---
name: pdm:plan-doc
description: Create a plan document and link it to the weekly log
argument-hint: <topic-name>
---

1. Accept $ARGUMENTS as topic name
2. Research the topic if needed (web search, codebase, MCP tools)
3. Create "[Topic] plan.md" in the notes vault root. Do not start with an H1 (Obsidian renders the filename as the title). Use this structure:
   - **Why** (problem/motivation)
   - **What** (scope and approach)
   - **Steps** (numbered, with owner where known)
   - **Risks and mitigations** (table)
   - **References** (links)
4. Find current week's log, add a linked TODO under the current day:
   - [ ] [Topic]: [[Topic plan]] #todo #workflow
5. Present for review
