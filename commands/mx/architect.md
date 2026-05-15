---
name: mx:architect
description: Analyze a problem domain and produce architecture, design, and planning artifacts
argument-hint: <problem description>
---

# Analyze and consider the problem: $ARGUMENTS

## Possible artifacts

- ARCHITECTURE.md for the data flow, major interfaces, and components.
- DESIGN.md to outline the user facing interfaces (UI, command line interfaces, file formats).
- LIBRARIES.md to outline the libraries used in the project and why
- PLAN.md to outline a phased approach to implementing the described framework.

## Follow these steps

1. Consider the request, use AskUserQuestion until the problem domain Is clear, and summarize potential approaches.
2. Once an approach is found, continue to use AskUserQuestion to define detail to produce the needed artifacts.
3. Plan the components, libraries, and general flow. Log all architectural decisions and patterns in ARCHITECTURE.md.
4. Provide a final review of all generated artifacts and identify any consistency differences and gaps.
5. If artifacts were created or changed and you're on a working branch (not `main` or `master`), commit locally:
   - Run `git status`.
   - Stage only the artifact files you intentionally changed.
   - Commit with a HEREDOC message using a `docs:` prefix (e.g., `docs: architecture for X`).
   - Do not push. Do not amend prior commits.
   - If you're on `main` or `master`, skip the commit and tell the user.
