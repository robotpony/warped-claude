---
name: gdoc-pull
description: Pull a Google Doc into the Obsidian vault via MCP
argument-hint: <document name>
---

# Pull a Google Doc into the vault

Pull "$ARGUMENTS" from Google Drive into the Obsidian vault using the `pull` MCP tool.

## Steps

1. Call the `pull` tool with `query` set to the document name provided above.
2. If multiple matches are returned, show the list and ask which file to pull. Then call `pull` again with the exact `path`.
3. Report the result: vault path, Drive URL (if available), and any section filter results.

## Notes

- The `pull` tool handles everything: search, download, HTML→markdown conversion, frontmatter injection, and sync state update.
- To pull only specific sections, call `pull` with a `sections` array of heading names or numeric indices.
- The file is written under the vault's configured `vaultRoot` directory (default: `gdrive/`), mirroring the Drive folder structure.
