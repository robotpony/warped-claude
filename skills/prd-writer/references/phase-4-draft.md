# Phase 4: Draft

Generate the outline, get approval, write the full PRD, self-review, present.

---

## Step 1: Read references

Before drafting, read these files:
- `references/prd-template.md` — the structural contract
- `references/writing-prompt.md` — tone and style
- `references/calibration-guide.md` — rules to prevent known V0 errors

Also read the research synthesis from `prd-drafts/{slug}/research-synthesis.md`.

If prior specs were researched, also read `prd-drafts/{slug}/research/prior-specs.md` directly. The synthesis may dilute breaking-change details — the drafting agent needs the raw prior-spec findings to flag contradictions accurately.

## Step 2: Generate outline

Produce a ~20-line skeleton: section headers + one-sentence summary per section.

The outline must:
- Follow the template structure exactly
- Map scope bullets to specific feature sections
- Include all mandatory sections from the template

Save to `prd-drafts/{slug}/outline.md`.

Present to the user: **"Here's the structure. Adjust before I draft?"**

### If user removes a mandatory section:
Push back with explanation. "The template requires [section] because [reason]. Are you sure you want to drop it?"

### If user adds a section not covered by research:
Accept it. Draft it thin with an explicit flag: `<!-- needs-additional-research: [what's missing] -->`. Don't re-enter the research phase.

Wait for approval.

## Step 3: Full draft

Write the complete PRD in one pass. One agent, not section-by-section — this ensures cross-references and tone consistency.

The drafting agent reads:
- The approved outline
- The research synthesis (Decisions & Constraints section has highest priority)
- `references/prd-template.md` (structural contract)
- `references/writing-prompt.md` (tone guide)
- `references/calibration-guide.md` (V0 error prevention)

Apply the 11 calibration rules during drafting:
1. Check for platform-wide behavior consistency
2. Use concrete tables, not abstract descriptions
3. Flag breaking changes from prior specs
4. Bias toward fewer requirements
5. Separate what from how
6. Keep JTBD readable by non-engineers
7. Include links to all sources
8. Number all lists
9. Put General Requirements first (with GR prefix: GR1, GR2, etc. — distinct namespace from Feature requirements)
10. (Spec splits come after draft is complete)
11. Include concrete performance thresholds where applicable (latency budgets, row ceilings, degradation factors)

Save to `prd-drafts/{slug}/prd.md`.

## Step 4: Silent self-review

Read `references/quality-rubric.md`. Score each of the 7 dimensions.

For any **Yellow:** attempt to fix. If you can't fix without more information, add a `<!-- needs-additional-research: [what's missing] -->` flag.

For any **Red:** must fix before presenting.

Do NOT show scores to the user. The self-review is silent.

After self-review fixes, save the improved version to `prd.md` and snapshot to `prd-v0.md`.

## Step 5: Present the draft

Tell the user: **"The draft is ready for your review."**

If any sections have `<!-- needs-additional-research -->` flags, surface them:
**"Heads up: [N] sections are thin because research didn't cover them. You'll want to address these during editing: [list sections]."**

The PRD is on disk at `prd-drafts/{slug}/prd.md`.

Add `<!-- phase-complete -->` to the bottom of `prd.md`.

---

## Offered: Collaborative Edit

After presenting the draft, offer: **"Ready for your feedback. Go section-by-section, dump everything at once, or just start editing — whatever works."**

Provide review prompts as suggestions, not a forced workflow:
- Goal: Does this map to one operational action?
- Metrics: Are baselines real? Any vanity metrics hiding?
- JTBD: Would a non-engineer follow these answers?
- Requirements: Any that are over-engineering? Any that are actually non-goals?
- Guardrails: Are thresholds numeric and specific?

Apply edits to `prd.md` on disk. The user drives.

## Offered: Language Polish

After editing settles, offer: **"Want me to do a language refinement pass?"**

If yes:
1. Save `prd-pre-polish.md` as snapshot
2. Show 3 before/after examples for approval
3. If approved, run a polish agent with instructions: **never alter meaning, only tighten language and fix tone consistency**
4. Apply to `prd.md`

## Offered: Spec Splits

After editing, suggest 2-4 smaller specs:
- Which features group together
- What overlapping research each split needs
- In what order they should be built

This helps the user break the big PRD into actionable chunks.
