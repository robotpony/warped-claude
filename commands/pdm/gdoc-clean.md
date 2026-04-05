---
name: pdm:gdoc-clean
description: Clean up a pasted Google Doc (structure and formatting)
argument-hint: <file-path>
---

1. Accept $ARGUMENTS as file path
2. Read the file, count lines and links
3. Ask scope: structure-only, structure + light editorial, or full restructure
4. Apply:
   - Strip Google Docs artifacts (double spacing, stray **, empty headers, orphan whitespace)
   - Convert flat numbered lists to nested markdown bullets
   - Normalize section headers and separators
   - Preserve all content, links, dates, status markers
5. Report: lines before/after, headers preserved (grep ###), links preserved (grep http)
