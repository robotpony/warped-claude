---
name: mx:ideate
description: Brainstorm approaches, outline requirements, and produce planning artifacts
argument-hint: <idea or problem to explore>
---

# Ideate and brainstorm: $ARGUMENTS

## Possible artifacts

- IDEAS.md
- OUTLINE.md

## Follow these steps

1. Consider the request, use AskUserQuestion until the problem domain Is clear, and summarize potential approaches. Suggest alternatives, identify tradeoffs, and look for consistency  issues. 
2. Once an approach is found, continue to use AskUserQuestion to define detail to produce the needed artifacts.
3. Outline the domain and general approach, in terms of requirements and motivations.
4. Provide a final review of all generated artifacts and identify any consistency differences and gaps.
5. If artifacts were created or changed and you're on a working branch (not `main` or `master`), commit locally:
   - Run `git status`.
   - Stage only the artifact files you intentionally changed.
   - Commit with a HEREDOC message using a `docs:` prefix (e.g., `docs: ideas for X`).
   - Do not push. Do not amend prior commits.
   - If you're on `main` or `master`, skip the commit and tell the user.
