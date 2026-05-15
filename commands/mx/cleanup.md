---
name: mx:cleanup
description: Clean up a project: dead code, docs, consistency, and version updates
argument-hint: <cleanup scope>
---

# Clean up task: $ARGUMENTS

Follow these steps:

1. Consider the request, use AskUserQuestion until the tasks are clear, and provide clear plan for review.
2. Set up the working branch:
   - Run `git branch --show-current`.
   - If on `main` or `master`, create `chore/cleanup-<short-kebab-slug>` and switch to it.
   - Otherwise stay on the current branch.
3. Review the README and changelog for accuracy and consistency, and apply the user's standard styles using the available writing guidelines.
4. Ensure the package version is up to date.
5. Review documentation for unused or extraneous files.
6. Review the code for unused and dead code, duplicated code, unsafe code, and inconsistencies.
7. Present a plan before continuing.
8. Once completed, ensure the package version is updated, build the project, update the changelog and README.
9. Commit locally:
   - Run `git status` and review what changed.
   - Stage only the files you intentionally changed (no `git add -A`).
   - Skip anything that looks like a secret or large binary.
   - Commit with a HEREDOC message using a `chore:` or `refactor:` prefix.
   - Do not push. Do not amend prior commits.
