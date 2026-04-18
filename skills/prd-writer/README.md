# /prd — PRD Writer Skill

Writes product requirements documents in Stas's format: decision-first, failure-aware, operationally grounded.

## Installation

Copy to your Claude Code skills directory:

```bash
cp -r prd-writer/ .claude/skills/prd-writer/
```

## Usage

Trigger with `/prd` or by asking to write a PRD.

## Phases

1. **Orient** — Fresh or continuing? Find existing drafts.
2. **Research** — Parallel agents explore codebase, external docs, prior specs.
3. **Scope** — Present 3-4 MVP options, user picks one, expand into scope bullets.
4. **Draft** — Outline checkpoint, full draft, silent self-review.
5. *(Offered)* **Collaborative Edit** — User-driven review and revision.
6. *(Offered)* **Language Polish** — Tighten language without altering meaning.
7. *(Offered)* **Spec Splits** — Suggest how to break the big PRD into smaller specs.

## Output

PRD artifacts are written to `prd-drafts/{topic-slug}/`:

```
prd-drafts/{topic-slug}/
  research-synthesis.md     — Decisions & Constraints at top, findings below
  research/                 — Raw agent outputs
    codebase.md
    external-docs.md
    prior-specs.md
  outline.md                — 20-line skeleton, approved by user
  prd.md                    — The live PRD
  prd-v0.md                 — First draft snapshot
  prd-pre-polish.md         — Snapshot before language pass (if polish was run)
```
