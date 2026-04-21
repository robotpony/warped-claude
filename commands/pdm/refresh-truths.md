---
name: pdm:refresh-truths
description: Check source-of-truth files for staleness and coverage gaps
argument-hint: [file-name or "all"]
---

Review source-of-truth files for staleness and missing coverage. Lightweight by default: flag and suggest, don't auto-edit.

## Source of Truth Registry

Read the Source of Truth Documents table in `CLAUDE.md` to get the current list of authoritative files. The table is the canonical registry; this command works against whatever is listed there.

## Input

1. Accept $ARGUMENTS as a file filter. Default to "all". If a specific file name or keyword is given (e.g., "glossary", "ontology", "priorities"), scope to that file only.

## For Each Source-of-Truth File

### 1. Staleness check

2. Run `git log -1 --format="%ai" -- "<file-path>"` to get the last modified date.
3. Flag files not updated in the last 4 weeks as potentially stale.
4. For the glossary specifically, check whether any terms have empty or stub definitions.

### 2. Coverage scan

5. Find the 3 most recent weekly logs in `log/2026/`.
6. Scan those logs and any `[[linked]]` docs referenced in them for:
   - **Glossary gaps:** Terms, acronyms, or codenames used in recent notes that don't appear in `Northbeam Glossary.md`. Focus on capitalized acronyms (3+ letters) and internal product names. Ignore common English words and well-known industry terms already in the glossary.
   - **Ontology gaps:** Platform names, channel labels, or tactic names referenced in recent notes that aren't captured in the labeling ontology.
   - **Priorities drift:** Projects or initiatives discussed in recent logs that don't appear in the development priorities doc (may indicate new work not yet added to the roadmap).

### 3. Cross-reference check

7. For the glossary, check for terms that reference each other but use inconsistent names (e.g., a term listed under one AKA but referenced elsewhere under a different alias).

## Output

Present findings grouped by file:

```
## Source of Truth Refresh — <date>

### <File Name>
- **Last updated:** <date> (<N weeks ago>)
- **Staleness:** OK / Stale (not updated in 4+ weeks)
- **Gaps found:** <count>
  - <term/item> — seen in <log file>, not in <source file>
  - ...
- **Consistency issues:** <count> (if any)
  - <description>
```

At the end, provide a summary:

**Healthy:** Files with no issues.
**Needs attention:** Files that are stale or have gaps.

## Rules

- Do not edit source-of-truth files automatically. Present findings and let the user decide what to update.
- When suggesting new glossary terms, include a draft definition based on usage context, but mark it as a suggestion.
- Keep the output scannable. If there are more than 10 gaps for a single file, show the top 10 and note the remainder.
- The gdrive/ files are synced copies. If they're stale, note that a re-sync from Drive may be needed rather than direct edits.
