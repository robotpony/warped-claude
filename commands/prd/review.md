---
name: prd:review
description: Review a PRD for quality, coherence, and readiness — from Notion, Obsidian, or pasted content
argument-hint: <notion-url-or-vault-path>
---

Review a PRD for structural quality, decision coherence, and shipping readiness. Works with Notion pages, vault files, or content pasted into the conversation. On subsequent passes within the same session, automatically detects prior reviews and reports only what changed.

## Input

1. Parse `$ARGUMENTS` for a source:
   - **Notion URL** (contains `notion.so` or `notion.site`): Extract the page ID. Fetch with `notion-fetch` (include_discussions: true), then `notion-get-comments` (include_all_blocks: true). Capture both page content and all comment threads.
   - **Vault file path**: Read the file with the Read tool.
   - **No argument**: Use content already present in the conversation. If no PRD content is visible, ask the user to provide it.

2. Read `~/.claude/rules/product-writing-rules.md` for the review standard. This is the primary quality rubric.

## Delta detection

3. Check conversation history for prior `prd:review` output in this session.
   - If a prior review exists, note which issues were previously flagged.
   - In the output, mark each issue as **New**, **Persists**, or **Resolved** relative to the last pass.
   - If this is the first pass, skip delta markers.

## Review

Run all seven checks against the document. Weight findings by impact on shipping readiness, not by count.

### 1. Structure check

Compare the document's sections against the PRD required sections from the product writing rules:
- Summary (2-3 sentences: what and why)
- Problem (what's broken or missing)
- Solution (what we're building)
- Success criteria (how we measure success)
- Scope (what's in, what's explicitly out)
- Open questions (what we still need to resolve)

Missing required sections are blocking issues. Variation in naming or ordering is fine if the content is present. Documents may include additional sections (user stories, technical constraints, rollout plan, etc.) and these don't need to map 1:1 to the template.

### 2. Decisions coherence

Read all stated decisions, constraints, and commitments across the entire document. Check:
- Do any decisions contradict each other?
- Does the scope section match what the solution section describes?
- Are decisions internally consistent (no sentence that says X followed by a sentence that says not-X)?
- Are decisions attributed to someone or a group?

### 3. Comment-vs-document drift (Notion sources only)

Compare active comment threads against the document body:
- Are there comments requesting changes that haven't been made?
- Are there resolved comments whose resolutions introduced new inconsistencies?
- Are there stale threads (discussion settled, but comment not resolved)?

Skip this check for non-Notion sources.

### 4. Open question severity

For each open question in the document:
- Can this ship without answering it? (Non-blocking)
- Does answering it change the solution design? (Blocking)
- Is it actually answered elsewhere in the document? (Stale; should be removed or moved to decisions)

### 5. Phase plan assessment

If the document includes phases, milestones, or a rollout plan:
- Are phases sequenced logically (dependencies flow forward)?
- Is each phase independently shippable?
- Are phase boundaries clear (what's in each phase, what's explicitly not)?
- Does the phase plan match the scope section?

If no phasing exists, note whether the scope suggests phasing would help (multiple user types, sequential dependencies, risk that warrants incremental delivery).

### 6. Spec and wireframe consistency

If the document includes wireframes, diagrams, feature tables, or detailed specs:
- Does the spec match what the prose describes?
- Are there features mentioned in prose but missing from tables (or vice versa)?
- Do wireframe labels match the terminology used in the document?

### 7. Copy review

Check against product writing rules anti-patterns:
- Corporate jargon ("leverage," "synergize," "alignment")
- Hedge stacking ("might potentially consider possibly")
- Passive responsibility ("it was decided")
- Vague timelines ("soon," "in the near future")
- Sections that outgrew their purpose
- Em-dashes (should use commas, semicolons, or separate sentences)
- Canadian English spelling (colour, favour, organize, optimize)

Also check the review checklist from the writing rules:
1. Can someone understand the main point from the first paragraph?
2. Are there sections that could be cut without losing value?
3. Are trade-offs named explicitly?
4. Are timelines and ownership specific?
5. Are there undefined terms?
6. Do we state what we don't know?
7. Is there a clear next action or decision?

## Output

Write the review directly in the conversation. Use this structure:

### Verdict

One sentence: is this ready to ship, close to ready, or needs significant work?

### Issues

List every finding, grouped by severity:

**Blocking** (cannot ship without resolving)
- `[Check name]` Issue description. If the fix is clear, include a concrete suggestion.

**Should fix** (weakens the document but doesn't block shipping)
- `[Check name]` Issue description. Suggest an edit if there's clarity on what it should say.

**Nits** (style, clarity, or polish)
- `[Check name]` Issue description.

For delta reviews, prefix each issue with **New**, **Persists**, or **Resolved**.

If a check finds no issues, omit it from the output. Don't list checks that passed.

### Phase plan

If phasing exists, include a brief assessment (3-5 lines). If the review suggests phasing should be added, say so here.

### Resolved since last pass (delta reviews only)

List issues from the prior review that are now fixed. Keep it brief.

## Rules

- Flag issues first. Only suggest specific edits when the fix is clear and unambiguous.
- Don't rewrite sections wholesale. Point to the problem and let the author fix it.
- Don't pad the review. If the document is strong, say so and keep the output short.
- Lean on the product writing rules as the quality standard, but don't be rigid about section naming or ordering. Content matters more than template conformance.
- No em-dashes in your output.
- Canadian English spelling.
- Don't suggest adding sections or content that the author may have intentionally omitted. Flag gaps as questions, not directives.
