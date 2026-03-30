# PRD Calibration Guide

How to produce a V0 that's closer to the collaboratively edited final. Based on the Google Incrementality PRD — 39 changes across 8 categories between V0 and final.

---

## 11 Rules

### 1. Ask about platform-wide behaviors BEFORE writing
The V0 assumed label versioning/snapshotting. Northbeam uses live labels everywhere — changes recalculate history. This reversed an entire feature.

**Rule:** During research, always ask: "What are the platform-wide behaviors this feature must be consistent with? (e.g., how do labels work across the product? Are things snapshotted or live?)"

### 2. Build concrete mapping tables DURING research, not drafting
The V0 described mapping rules abstractly. The final had a full table showing every combination, which revealed an overlap the V0 completely missed.

**Rule:** If the feature involves mapping/configuration rules, build the actual table during research and show it to the user before writing. The table IS the research artifact.

### 3. Compare new requirements against prior spec commitments
The V0 didn't flag a breaking change from an earlier spec. The final caught it and called it out explicitly.

**Rule:** If prior specs exist, read them during research and flag any requirement that contradicts or changes a prior commitment. Mark it with "**Breaking change from [spec name]:**"

### 4. Bias toward fewer requirements, not more
The V0 had 6 features with 25+ requirements. The final consolidated to 5 features. Requirements that were over-engineering were either deleted or marked as non-goals.

**Rule:** For each requirement, ask: "Is this solving a real problem we've seen, or preventing a hypothetical one?" If hypothetical, make it a non-goal or cut it.

### 5. Separate what-to-achieve from how-to-implement
The V0 specified SQL CTE details, formula names, and file paths. The final kept behavioral requirements and let eng choose implementation.

**Rule:** Requirements describe WHAT the system does, not HOW. Implementation details are acceptable only as reference examples when they clarify a point.

### 6. Run a non-engineer readability test on JTBD
The V0 used jargon a media buyer couldn't follow.

**Rule:** After writing JTBD, have an agent with no domain context read the Q&As from a non-technical persona. Rewrite anything they can't follow.

### 7. Always include links to source materials
The V0 had zero links. The final had 6+ links to Notion specs, Google Sheets, Figma files, and a prototype PR.

**Rule:** The Research section must contain clickable links to every source mentioned. If a source isn't linkable, describe where it lives.

### 8. Use numbered lists everywhere
The V0 mixed bullets and numbers. The final uses numbers for everything.

**Rule:** All lists are numbered. This lets people reference "Non-Goal 7" or "F2 Guardrail 3" in discussion.

### 9. General Requirements come FIRST
The V0 put General Requirements after all features. The final moves them before Feature 1 — they set the baseline.

**Rule:** General Requirements section appears first under Requirements, before any Feature section.

### 10. Suggest spec splits after writing
The big PRD is the right first artifact, but it's too much to read at once.

**Rule:** After the V0 is complete, recommend how to split it: which features group together, what overlapping research each split needs, and in what order they should be built.

### 11. Requirements with performance implications need a concrete threshold
The V0 had a monitoring alert for "CTE performance degradation" — vague. The final specified "per-adkey bucket resolution adds no more than 200ms latency for clients with <10,000 Google adkeys."

**Rule:** When a requirement has performance implications, include a number: latency budget, row count ceiling, or acceptable degradation factor. "Should be fast" is not a requirement.

---

## 3 Additional Patterns

### State machines
When a feature involves entities moving through states, define the states and transitions explicitly. Use a table:

| State | Entry condition | Valid transitions | Exit condition |
|-------|----------------|-------------------|----------------|
| Draft | Created by user | → Active, → Deleted | User activates or deletes |
| Active | User activates | → Paused, → Completed | Pause trigger or completion criteria |

Don't describe state behavior in prose — the table IS the requirement.

### Scenario-based failure handling
When a process can produce multiple inconclusive or partial outcomes, label each scenario and define: what happened, what this means, what to do next.

Example pattern:
> **Scenario B: Test ran but result is inconclusive**
> - What happened: Confidence interval includes 1.0x
> - What this means: Can't distinguish from no effect
> - What to do next: Extend test duration or increase budget

### Mutually exclusive states with numeric ranges
When a value maps to exactly one state, define ranges with no gaps and no overlap:
- **On Track:** ≥90%
- **Partially On Track:** 70-89%
- **Off Track:** <70%

Every possible value must resolve to exactly one state.

---

## Kill/Demote Examples

### Requirement killed → Non-Goal
**Before (V0):** "R8: The system stores mapping rule history. When rules change, the previous version is archived with a timestamp."
**After (Final):** "NOT versioning mapping rules. Label changes recalculate everything including history — this is consistent with how Northbeam handles labels everywhere."

Why: The requirement assumed snapshotting. The platform uses live recalculation. The requirement contradicted a platform-wide behavior.

### Requirement demoted to Stretch
**Before (V0):** "R23: The settings page shows campaign count and spend per bucket, with a coverage percentage."
**After (Final):** "F2 R7: Coverage report shows mapping rules and empty-bucket flags only. Campaign count and spend per bucket **(Stretch)**."

Why: Rich diagnostics were over-engineering for MVP. The core need is "are my buckets configured?" not "how much spend flows through each?"

### Requirement deleted entirely
**Before (V0):** "R9: Weekly automated coverage report sent to account stakeholders."
**After (Final):** Deleted. Not in Non-Goals, not in Stretch. Gone.

Why: No one asked for it. It was preventing a hypothetical problem. Cut it.

---

## Tone Calibration

### Too verbose:
> "When a customer runs a Google Branded Search test and gets a 0.6x multiplier, the system must apply that multiplier only to campaigns that resolved to the Google Ads - Branded Search bucket at query time, while all other Google campaigns continue to use their existing multipliers or pass through at face value (1.0x implicitly)."

### Right tone:
> "The system resolves each campaign to its bucket and applies that bucket's multiplier. Campaigns not covered by any test pass through unchanged."

State the behavior. Trust the reader.

### Non-Goals — too complex:
| Non-Goal | Why It's Tempting | Why It's Dangerous |
|----------|---|---|

### Non-Goals — right format:
1. **NOT doing X.** One sentence explanation.

Bold the boundary. One sentence of context. Three columns is over-engineering the format.
