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
    - If yes, write a draft in `release-notes/` using the Slack announcement format below.
    - Flag items that need screenshots or visual examples.

## Slack announcement format

The release note draft is a Slack-ready announcement. Follow this structure:

```
:mega: <Theme headline> :mega:

<One-sentence intro summarizing what shipped.>

**<Metric or feature name>.** <Plain-English definition: what it measures, how it works, and what date range or scope applies. One to two sentences.>
**<Next metric or feature>.** <Definition.>
...

**Availability and scope:**

<Where these are available (which pages, exports, plans). Any important caveats about methodology, windows, or cohorts. One short paragraph.>

Kudos to <@-mentions of contributors>
```

Rules for the format:
- Group items by theme. If a batch spans multiple themes (new metrics, bug fixes, UX improvements), use separate `:mega:` blocks or bold section headers within one post.
- Each item gets a **bold name** followed by a period, then a customer-facing definition. No bullet points for individual metrics; use line breaks.
- Definitions describe *what the customer sees*, not implementation details (no feature flag names, no internal field names).
- The "Availability and scope" section states where to find the feature and any important caveats.
- Kudos line at the end @-mentions contributors from the ClickUp assignees and watchers.
- Bug fixes use past tense: "**Custom metrics in table customization.** Fixed an issue where..."

## Notes

- "Release notes" is a done-type status in ClickUp — these tasks are complete, just awaiting documentation.
- Focus on customer impact, not engineering details.
- If a task has no description, include it but mark it: `(needs detail from <assignee>)`
