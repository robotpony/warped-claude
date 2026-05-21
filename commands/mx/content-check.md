---
name: mx:content-check
description: Audit Markdown docs against the user's writing rules and report a punch list of issues
argument-hint: <path or glob> [--fix] [--ruleset product|blog|recipe]
---

# Content check: $ARGUMENTS

Audit prose in the target file(s) against the user's writing rules. Report findings as a punch list; do not auto-edit unless `--fix` is passed.

## Inputs

Parse `$ARGUMENTS` for:
- **Path** (required): a file, directory, or glob. Examples: `README.md`, `docs/`, `authored/**/*.md`. If omitted, default to all top-level docs (`README.md`, `CLAUDE.md`, `INSTALL.md`) plus `docs/*.md`, excluding `_templates/`, `_preview-data/`, `_index/`, `node_modules/`, `.venv/`, and anything under `tools/build_catalog/tests/fixtures/`.
- **`--fix`** (optional): apply only the safe mechanical fixes listed under "Auto-fixable" below. Skip everything else and leave it in the report.
- **`--ruleset`** (optional): `product` (default for `README`, `CLAUDE`, `docs/*`, `INSTALL`), `blog`, or `recipe`. If omitted, infer per file: recipes under any `recipes/` dir get `recipe`; blog posts get `blog`; everything else gets `product`. The `authored/STYLE_GUIDE.md` uses `product`.

## Rulesets

Load the matching skill from `~/.claude/skills/`:
- `product` → `product-writing/SKILL.md`
- `blog` → `blog-writing/SKILL.md` (plus `blog-writing/reference.md` for context only)
- `recipe` → `recipe-writing/SKILL.md`

If a skill file is missing, report it and continue with the others.

## Checks

For each file, scan for:

1. **Em-dashes and en-dashes** — flag every occurrence. For `product` and `recipe` rulesets, suggest comma, semicolon, or colon. For `blog`, allow up to 2 per post; flag the rest.
2. **"Not X, but Y" structures** — phrases matching `not (just |only |simply )?\w+[,.]?\s+(but |it'?s\s+)`.
3. **Transition crutches** — paragraph- or sentence-leading `However,`, `Moreover,`, `Furthermore,`, `In addition,`, `On the other hand,`. Flag if more than 2 per document.
4. **Vague intensifiers and filler** — `one of the most`, `it'?s crucial`, `it'?s important`, `plays an? \w+ role`, `key factor`, `extensive(ly)?`, `comprehensive(ly)?`, `significantly`.
5. **Corporate jargon** — `leverage`, `synergy|synergi[zs]e`, `stakeholder(s)?`, `buy-in`, `circle back`, `take.*offline`, `best practices`, `learnings`, `actionable insights`, `move the needle`, `alignment` (when used as a noun about people).
6. **Symbolic inflation** — `stands as a testament`, `serves as a powerful reminder`, `represents a dynamic`, `at the heart of`, `in the ever-evolving`.
7. **Promotional drift** — `captivating`, `majestic`, `fascinating`, `seamlessly`, `delve`, `tapestry`, `vibrant`, `bustling`.
8. **Sycophantic openers** (lines starting a section) — `Great question`, `Fascinating`, `Excellent point`.
9. **Hedge stacks** — three or more hedges in one sentence (`might`, `possibly`, `perhaps`, `potentially`, `could`, `may`, `consider`).
10. **Rule-of-three overuse** — flag a document when 4+ Oxford-comma triples appear within 1000 words.
11. **Passive responsibility** — `it was decided`, `it has been determined`, `the decision was made`.
12. **Vague timelines** — `in the near future`, `soon`, `eventually`, `shortly` (when not in a code/quoted block).
13. **Meta-commentary** — `As we'?ve (explored|discussed|seen)`, `Building on this`, `To summarize`, `In conclusion,` at paragraph start.
14. **Bullet overload** — flag any file where bulleted/numbered lines exceed 60% of non-blank, non-header lines.
15. **Recipe-only** (when ruleset is `recipe`): backstory paragraphs before the Mechanic/Ingredients sections; "a knob of", "a pinch" outside finishing spices; future-tense or passive method steps (`should be added`, `you will want to`).

Ignore matches inside fenced code blocks, inline code spans, YAML frontmatter, blockquotes that cite external sources, and Obsidian wikilinks (`[[...]]`).

## Output format

Report as a single Markdown punch list grouped by file. For each finding:

```
### path/to/file.md
- L42 `em-dash` — "…faster tools — new fatigue" → suggest `,` or `;`
- L78 `vague-intensifier` — "It's crucial to understand that…" → prove with specifics
- L104 `not-x-but-y` — "It's not just speed, it's…" → state what it is directly
```

End with a per-rule tally and a per-file count, sorted worst first. No prose summary, no "key takeaways" section.

## Auto-fixable (with `--fix`)

Only these are safe to apply mechanically. Everything else stays in the report.

- Em-dash `—` between words with spaces on both sides → `, ` (or `; ` if the following clause begins with `but`, `so`, `and`, `or`). En-dash `–` used between words → same.
- Single straight quotes around obvious contractions left alone; do not touch quote style.
- `however,` at sentence start → leave the word, just flag (not auto-fix).

Do not auto-fix jargon, symbolic inflation, or anything structural. If `--fix` is passed:
1. Apply the safe fixes.
2. Show a diff summary (file + count of replacements per rule).
3. Still produce the full punch list for the remaining items.

## Steps

1. Resolve the target paths from `$ARGUMENTS`. If the resolution finds zero files, stop and ask the user for a path.
2. Pick the ruleset per file (default `product`, override with `--ruleset` or the path-based rules above).
3. Load the matching skill file(s) from `~/.claude/skills/`. Confirm load with a one-line note (`Loaded product-writing skill`).
4. Run the checks. Use `grep`/`rg` for the regex-shaped ones; read the file with `Read` for context-sensitive ones (bullet ratio, rule-of-three density, recipe structure).
5. Print the punch list. No commits, no edits, unless `--fix` was passed.
6. If `--fix` was passed: apply the safe fixes with `Edit`, then print the diff summary above the punch list. Do not stage or commit — leave the changes in the working tree for the user to review.

## Non-goals

- Do not change the meaning of any sentence.
- Do not bump the package version.
- Do not touch `CHANGELOG.md` unless the user explicitly says so.
- Do not edit files outside the resolved path set.
- Do not create new files or rewrite whole sections; this command finds issues, it does not rewrite prose.
