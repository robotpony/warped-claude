---
name: mx:finish
description: Finalize changes, update version/docs, and prepare for commit
argument-hint: <description-of-changes>
---

# Finalize the changes: $ARGUMENTS

1. Review all changed files for completeness and consistency
2. Run the build and verify it passes
3. Run tests and fix any failures
4. Update the package version (patch for fixes, minor for features)
5. Update the changelog with a summary of changes
6. Update the README if any public API or usage changed
7. Provide a commit message for manual commit
