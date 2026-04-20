---
name: vault:cleanup
description: Cleans up an Obsidian vault, using a checklist and rules encoded in Claude.md
argument-hint: <additional directions>
---

# Obsidian Vault Cleanup

Audit and organize the current Obsidian vault. Run from the vault root.

## Prerequisites

Read these files first to understand the vault's intended structure:

1. `README.md` — vault purpose and folder structure
2. `CLAUDE.md` — filing rules, naming conventions, and exceptions

If either file is missing, note it and proceed with reasonable defaults based on common Obsidian patterns.

## Audit phases

### 1. Structure discovery

Read the vault's documentation and build a mental model of:

- Folder purposes (what goes where)
- Naming conventions (date prefixes, slugs, etc.)
- File types and their expected locations
- Any explicit exceptions or edge cases

List the documented structure back to the user for confirmation before proceeding.

### 2. Empty file scan

Find files that are empty or contain only frontmatter with no content. For each:

- Report the file path and creation date
- Ask whether to delete, keep, or add a placeholder note

Present these in batches of 10-15 files. Don't delete without confirmation.

### 3. Misfiled notes

Compare actual file locations against the documented structure. Flag files that appear to be in the wrong folder based on:

- Content type vs. folder purpose
- Naming pattern mismatches
- Orphaned files in root or unexpected locations

For each misfiled file:

- Show current location and suggested destination
- Explain the reasoning
- Offer to move it

### 4. Folder-specific rule enforcement

For each folder that has explicit rules documented in CLAUDE.md, scan its files and validate them against those rules. Derive the checks from what CLAUDE.md actually specifies — don't assume a fixed set of rules. Common things CLAUDE.md may define:

- Required frontmatter fields and their formats
- Allowed tag values or taxonomies
- Naming conventions (slugs, date prefixes, case)
- Companion files that must stay in sync (e.g., a log or index file)
- Structural constraints (flat vs. sub-folders, one file per topic, etc.)

For each folder with documented rules:

1. List the rules extracted from CLAUDE.md for that folder
2. Scan files in that folder and check each rule
3. Report violations grouped by rule, showing affected files
4. For fixable issues (missing fields, wrong case, missing log entries), offer to apply the fix
5. For judgment calls (wrong tag, misnamed file), show the issue and ask

Skip folders with no folder-specific rules in CLAUDE.md. Skip `gdrive/` and dot-folders entirely.

Present findings folder by folder. Wait for direction before making any changes.

### 6. Structure gaps

Identify patterns that suggest missing rules or folders:

- Clusters of similar files without a home
- Repeated filing decisions that aren't documented
- Folders that have grown to need subdivision

Summarize findings and suggest specific additions to CLAUDE.md or README.md.

### 7. Rule improvements

Based on the audit, suggest updates to the vault's filing rules:

- New rules for recurring patterns
- Clarifications for ambiguous cases
- Deprecated rules that no longer apply

Present these as specific text to add or modify in CLAUDE.md.

## Output format

After each phase, summarize:

- What was found
- What actions are available
- What you recommend

Wait for user direction before taking action. Never delete or move files without explicit approval.

## Constraints

- Respect `.obsidian/` and other dot-folders (never modify)
- Preserve frontmatter and internal links when moving files
- Update any `[[wikilinks]]` that would break from moves
- Log all changes made for easy reversal
