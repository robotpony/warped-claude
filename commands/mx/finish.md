---
name: mx:finish
description: Finalize changes, update version/docs, and prepare for commit
argument-hint: <description-of-changes>
---

# Finalize the changes: $ARGUMENTS

1. Review all changed files for completeness and consistency.
2. Run the build and verify it passes.
3. Run tests and fix any failures.
4. Update the package version (patch for fixes, minor for features).
5. Update the changelog with a summary of changes.
6. Update the README if any public API or usage changed.
7. Commit locally on the current branch:
   - Run `git status` and `git branch --show-current`. If you're on `main` or `master`, stop and ask before committing — `mx:finish` is meant for finalizing on a working branch.
   - Stage only the files you intentionally changed (no `git add -A`).
   - Skip anything that looks like a secret or large binary.
   - Commit with a HEREDOC message that summarizes the work.
   - Do not push. Do not amend prior commits.
