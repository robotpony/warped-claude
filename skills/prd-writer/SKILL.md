---
name: prd-writer
description: "Write a product requirements document in Stas's format: decision-first, failure-aware, operationally grounded. Trigger when the user says 'prd', 'write a PRD', 'product requirements', 'spec', or asks to write requirements for a feature."
allowed-tools: "Read, Grep, Glob, Edit, Write, Bash, Agent"
---

# /prd — PRD Writer

Write product requirements documents in Stas's format. Four mandatory phases, three offered extensions.

## How this skill works

Short orchestrator. Per-phase instructions live in `references/phase-N-*.md`. Read only the current phase's reference file — never load all phases at once.

Output goes to `prd-drafts/{topic-slug}/` in the current working directory.

---

## Phase 1: Orient

Read `references/phase-1-orient.md` and follow its steps.

- Fresh or continuing? Glob `prd-drafts/*/` for existing PRDs.
- If continuing: show what artifacts exist, ask where to resume. Don't infer phase — the user knows.
- If fresh: get the topic, create the folder, confirm the research target codebase.
- One question at a time.

---

## Phase 2: Research

Read `references/phase-2-research.md` and follow its steps.

- Ask: "What codebase should I research?" Default: current working directory.
- Launch parallel agents to explore codebase + external docs + prior specs.
- Raw outputs saved to `prd-drafts/{slug}/research/`.
- Synthesis agent reconciles findings, flags contradictions.
- Second short agent catches what synthesis glossed over.
- Follow-up questions to user — one decision at a time.
- User decisions appended to Decisions & Constraints section at top of synthesis.
- Confidence markers: "**Confirmed in code:** [path]" and "**Uncertain:** [reason]".
- Checkpoint: "Here's what I found. What did I miss?"

**Context management:** Use agents for large docs. Keep main context clean.

---

## Phase 3: Scope

Read `references/phase-3-scope.md` and `references/mvp-options-format.md`.

- Present 3-4 MVP options (gold standard format) from narrowest to widest.
- User picks one.
- Expand into 5-8 specific scope bullets, confirm.
- Expanded scope written to Decisions section of synthesis.

---

## Phase 4: Draft

Read `references/phase-4-draft.md`. Also read `references/prd-template.md`, `references/writing-prompt.md`, and `references/calibration-guide.md`.

- Generate 20-line outline from research + scope + template.
- Checkpoint: "Here's the structure. Adjust before I draft?"
- If user adds sections not covered by research: draft thin with "needs additional research" flag.
- If user removes mandatory sections: push back with explanation.
- Full draft in one pass, reading outline + synthesis + references.
- Silent self-review against `references/quality-rubric.md` — fix Yellow/Red before presenting.
- Save `prd-v0.md`.
- Surface any thin sections: "Heads up: N sections are thin."
- Present: "The draft is ready for your review."

---

## Offered: Collaborative Edit

After presenting the draft:

- Provide review prompts as suggestions, not a forced workflow.
- User drives — section-by-section, batch feedback, or conversational.
- Edits applied to `prd.md` on disk.

---

## Offered: Language Polish

After editing settles:

- "Want me to do a language refinement pass?"
- Save `prd-pre-polish.md` as snapshot.
- Show 3 before/after examples, get approval before full pass.
- Agent instructions: never alter meaning, only tighten language.

---

## Offered: Spec Splits

After editing:

- Suggest 2-4 smaller specs with rationale.
- Which features group together, what research overlaps, build order.

---

## State is artifacts

No JSON state file. Phase 1 globs for existing files. Completion tracked by `<!-- phase-complete -->` at the bottom of each artifact. File without marker = incomplete.

## Output structure

```
prd-drafts/{topic-slug}/
  research-synthesis.md
  research/
    codebase.md
    external-docs.md
    prior-specs.md
  outline.md
  prd.md
  prd-v0.md
  prd-pre-polish.md
```
