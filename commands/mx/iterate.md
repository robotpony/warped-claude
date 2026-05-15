---
name: mx:iterate
description: Review feedback, update plans and documentation based on discussion
argument-hint: <feedback or direction>
---

# Review the user response: $ARGUMENTS.

Follow these steps:

1. Consider the request, use AskUserQuestion until the direction is clear, and provide an updated plan, by way of making recommendations and suggestions.
2. Continue the conversation and use of AskUserQuestion to agree on a set of directions and tradeoffs for the remaining work.
3. Update project definition, plans, and documentation based on the discussion.

The goal of iteration is to progress the current work forward with agreed changes, summarizing any changes in direction. The iteration does not include code changes, but the user can choose to progress to execute the documented plans.

If documents changed and you're on a working branch (not `main` or `master`), commit locally:
- Run `git status` and review the diff.
- Stage only the docs you intentionally changed (no `git add -A`).
- Commit with a HEREDOC message using a `docs:` prefix that names what was iterated on.
- Do not push. Do not amend prior commits.
- If you're on `main` or `master`, skip the commit and tell the user.