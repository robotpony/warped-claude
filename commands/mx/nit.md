---
name: mx:nit
description: Make a small quality-of-life improvement with a minor version bump
argument-hint: <improvement description>
---

# Analyze and improve: $ARGUMENTS.

Follow these steps:

1. Consider the request, use AskUserQuestion until the tasks are clear, and provide clear plan for review.
2. Set up the working branch:
   - Run `git branch --show-current`.
   - If on `main` or `master`, create `chore/<short-kebab-slug>` (2-4 words from $ARGUMENTS) and switch to it.
   - Otherwise stay on the current branch.
3. Make the change.
4. Once completed, ensure the package version is updated (minor version change only).
5. Build the project, update the changelog and README.
6. Commit locally:
   - Run `git status` and review what changed.
   - Stage only the files you intentionally changed (no `git add -A`).
   - Skip anything that looks like a secret or large binary.
   - Commit with a HEREDOC message that signals a quality-of-life improvement (e.g., `nit:` or `chore:` prefix).
   - Do not push. Do not amend prior commits.
