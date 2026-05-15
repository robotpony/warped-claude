---
name: mx:bug
description: Analyze and fix a bug with root-cause analysis and regression checks
argument-hint: <bug description>
---

# Analyze and fix: $ARGUMENTS

Follow these steps:

1. Clarify the bug using AskUserQuestion:
   - What's the observed behaviour vs. expected behaviour?
   - Can it be reproduced reliably? What are the steps?
   - When did it start? Was anything recently changed?

2. Set up the working branch:
   - Run `git branch --show-current`.
   - If on `main` or `master`, create `fix/<short-kebab-slug>` (2-4 words from the bug) and switch to it.
   - Otherwise stay on the current branch.

3. Reproduce and root cause:
   - Identify the minimal reproduction path.
   - Trace to root cause, not just symptoms.
   - Note any related code that might be affected.

4. Fix:
   - Make the smallest change that correctly addresses the root cause.
   - Avoid fixing unrelated issues in the same pass.

5. Regression check:
   - Confirm the fix resolves the original behaviour.
   - Check that no adjacent behaviour was broken.
   - Note if a test should be added.

6. Finalize:
   - Update the package version (patch bump).
   - Build the project.
   - Update the changelog and README if needed.

7. Commit locally:
   - Run `git status` and review what changed.
   - Stage only the files you intentionally changed (no `git add -A`).
   - Skip anything that looks like a secret or large binary; flag it instead.
   - Commit with a HEREDOC message that names the bug fixed, not just "fix bug".
   - Do not push. Do not amend prior commits.
