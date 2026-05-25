---
name: mx:feature
description: Implement a feature with tests, docs, and version updates
argument-hint: <feature description>
---

# Review the project and make the requested changes: $ARGUMENTS.

Follow these steps:

1. Consider the request, use AskUserQuestion until the tasks are clear, and provide clear plan for review.
2. Set up the working branch:
   - Run `git branch --show-current`.
   - If on `main` or `master`, use `EnterWorktree` with name `feat/<short-kebab-slug>` (2-4 words from $ARGUMENTS) — this creates an isolated worktree on a fresh branch so your main checkout stays untouched.
   - Otherwise stay on the current branch (no worktree).
3. Make the requested changes using standard coding techniques, including unit tests.
4. Once completed:
   - Mark the step or phase in the plan complete.
   - Ensure the package version is updated.
   - Build the project.
   - Update the changelog and README.
5. Commit locally:
   - Run `git status` and review what changed.
   - Stage only the files you intentionally changed (no `git add -A`).
   - Skip anything that looks like a secret or large binary; flag it instead of committing.
   - Commit with a HEREDOC message that names the feature.
   - Do not push. Do not amend prior commits.
