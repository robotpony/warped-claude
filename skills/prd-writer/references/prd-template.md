# PRD Template

> **Rule:** If a section can be filled in with generic language, it is not complete.

This is the structural contract. All phase reference files point here. Every PRD the skill produces follows this skeleton.

---

## Goal (Decision-First)

In 2-3 sentences:
- What _decision_ this project enables
- Who makes that decision
- How often they make it

> If this does not clearly map to an operational action, rewrite it.

No metadata header. No author/date/status/urgency. Git tracks that. The PRD starts here.

---

## Success Metrics (Decision Quality, Not Vanity)

| Goal Type | KPI | Target | Baseline | Why this KPI proves the decision is better |
|-----------|-----|--------|----------|---------------------------------------------|
| Adoption  |     |        |          |                                             |
| Engagement|     |        |          |                                             |
| Impact    |     |        |          |                                             |

Rules:
1. Every KPI must explain _why_ it proves improved decision-making
2. Avoid proxy metrics unless the true metric is impossible to observe
3. Baselines must be real numbers, not "N/A" or "TBD"

---

## Jobs-to-Be-Done (Questions, Not Tasks)

Frame as questions the product must answer:
- "How do I...?"
- "When should I...?"
- "What is working vs what just looks good?"

> If a question cannot be answered deterministically by the system, call it out.

Include a **Terminology** subsection if domain terms need defining for mixed audiences.

### Before / After (Operational Reality)

| Before (Today) | After (With This Shipped) |
|-----------------|---------------------------|
| What users do | What they stop doing |
| Where errors happen | How errors are prevented |
| How trust breaks | How trust is maintained |

Rule: "After" must include _fewer decisions_, not more knobs.

---

## Research & Inputs (Reality Check)

**Sources** — with clickable links to every source mentioned:
1. Internal data
2. Customer feedback
3. Prior incidents / failures
4. Comparable systems (internal or external)
5. Prior specs — link each one. Flag any breaking changes.

**Explicit gaps:**
1. What we _do not_ know yet
2. What assumptions this PRD relies on

> Unknowns that could invalidate decisions must be promoted to Open Questions.

---

## Non-Goals (Hard Boundaries)

Numbered list. Bold the boundary, one sentence of context.

1. **NOT doing X.** One sentence explanation.
2. **NOT doing Y.** One sentence explanation.

> If something is tempting but dangerous, list it here.
> Requirements that are actually non-goals should be deleted, not softened.

---

## Requirements

### General Requirements (Apply Everywhere) — FIRST, before features

Numbered list with **GR** prefix. These set the baseline that all features build on. The GR prefix gives them their own namespace — people reference "GR4" in discussion, distinct from "F1 R4".

1. **GR1.** Validation before execution
2. **GR2.** Clear system state visibility
3. **GR3.** Defaults chosen by the system, not the user
4. **GR4.** Bad configurations are blocked, not documented

### Feature Requirements (Repeatable Pattern)

#### Feature N: [Name]

**Purpose** — What failure or ambiguity this feature prevents.

**Requirements** — numbered:
1. R1: ... **(Stretch)** if applicable — mark inline, don't separate
2. R2: ...
3. R3: ...

**Guardrails** — numbered:
1. Defaults: ...
2. Allowed ranges: ...
3. Blocking conditions: ...
4. Warnings (allowed but risky): ... — mark eng notes as "(eng note)"

Rules:
- Requirements describe WHAT the system does, not HOW
- Implementation details are acceptable only as reference examples
- For each requirement ask: "Is this solving a real problem or preventing a hypothetical one?"
- If hypothetical → make it a non-goal or cut it
- All lists are numbered so people can reference "F2 R3" or "F1 Guardrail 2" in discussion
- If the feature involves mapping/configuration, build the actual table — show ALL relevant facets as columns, call out overlaps and gaps at the point of decision
- Reuse existing UX patterns explicitly — name them, say "reuse this, do not build new"

---

## Monitoring & Alerts

Table format when applicable:

| Alert | Trigger | Severity | Remediation |
|-------|---------|----------|-------------|

---

## Integrity Risks

Name the 2-3 most dangerous scenarios where the system could produce a result that looks correct but leads to a bad business decision. For each: how it happens, how to mitigate.

---

## Open Questions

Numbered list. Each with a clear recommendation if possible.

1. **[Question]** — Recommendation: ...

---

## Formatting Rules

1. All lists are numbered — Non-Goals, Guardrails, Requirements, everything
2. General Requirements use **GR** prefix (GR1, GR2, etc.) — own namespace, distinct from Feature requirements (F1 R1, F2 R3, etc.)
3. No metadata header (author/date/status)
4. Stretch items marked inline with **(Stretch)**
5. Eng implementation notes marked with **(eng note)**
6. Confidence markers in research: "**Confirmed in code:** [path]" and "**Uncertain:** [reason]"
7. `<!-- phase-complete -->` marker at the bottom of each completed artifact
