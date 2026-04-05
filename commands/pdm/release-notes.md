---
name: pdm:release-notes
description: Pull completed tasks needing release notes from ClickUp and format for the weekly log
argument-hint: [board-or-list-name]
---

Pull tasks in "release notes" status from ClickUp and prepare them for drafting.

## Gather

1. If $ARGUMENTS specifies a board/list name, search that list. Otherwise default to the **NB3.0 Iterations** list in the MTA folder.
2. Search ClickUp for tasks in "release notes" status on the target list using `clickup_search` or `clickup_filter_tasks`.
3. For each task found, fetch full details with `clickup_get_task` to get:
   - Task name, description, and acceptance criteria
   - Assignee(s)
   - Priority
   - Tags
   - Linked tasks or dependencies
   - Attachments (note them, don't fetch)

## Summarize

4. For each task, produce a one-line release note summary:
   - **What changed** from the customer's perspective (not implementation details)
   - If the description is empty or too technical, flag it for human input
   - Group by theme if multiple tasks relate to the same feature area

5. Format as a checklist ready for the weekly log:

```
- [ ] <Customer-facing summary> ([<task-id>](<clickup-url>), <assignee>)
```

## Insert into weekly log

6. Find the current weekly log in `log/2026/`.
7. Locate the `# Release notes` section.
8. If items already exist there, append new ones below. Don't duplicate tasks already listed (match by ClickUp task ID).
9. Present the additions to the user before writing.

## Optionally draft release note copy

10. Ask: "Want me to draft customer-facing release note copy for these?"
    - If yes, write a draft in `release-notes/` using the format of existing files in that folder.
    - Each item gets: a short headline, 1-2 sentence description of what changed and why it matters, and any relevant context.
    - Flag items that need screenshots or visual examples.

## Notes

- "Release notes" is a done-type status in ClickUp — these tasks are complete, just awaiting documentation.
- Focus on customer impact, not engineering details.
- If a task has no description, include it but mark it: `(needs detail from <assignee>)`
