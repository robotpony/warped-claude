---                                                                                                         
name: pdm:carry-forward         
description: Carry forward this week's notes and tasks
argument-hint: [additional-instructions]
---   

1. Find the current week's log
2. Read it + all linked docs
3. Separate TODOs into:
   - **Done** (`- [x]`): leave in the current log. **Never carry these forward**, including items completed but tagged `*(carried)*`, `#todone`, or with a `@date`.
   - **Rolling forward** (`- [ ]` only): extract these for next week.
   - **Dead**: items that appear stale (carried 2+ weeks with no progress noted). Flag for user decision; do not carry silently.
4. In the **previous** week's log, tag every carried-forward item with `#moved` so it's
   clear the item migrated. (Add `#moved` at the end of the line; don't change the
   checkbox state.)
5. Clean up the Context section: remove anything now in linked docs, keep orphaned threads
6. Group rolling TODOs by workstream (diagnostics, search, benchmarks, other)
7. Write a # Carry-forward for week of [date] section at the bottom with:
   - Grouped open TODOs (all `- [ ]`, never `- [x]`)
   - "What got done" summary
8. Carry open threads from the Context section into the carry-forward:
   - Include threads that are unresolved (no linked doc, no decision noted)
   - Drop threads that now have a dedicated linked doc
9. **Verify** the carry-forward block contains zero `- [x]` items before presenting or seeding the next file.
10. Optionally seed next week's log file with the carry-forward TODOs + context     