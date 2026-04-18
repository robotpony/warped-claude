# PRD Quality Rubric

> **Purpose:** Evaluate PRDs on decision quality, not completeness. Each dimension scores **Green / Yellow / Red**.

Used during Phase 4 silent self-review. Fix Yellow/Red before presenting to user.

---

## 1. Goal & Decision Clarity

- **Green:** One decision, one owner, one cadence.
- **Yellow:** Decision exists but is buried or split across paragraphs.
- **Red:** "Insights" without action. Multiple implied decisions.

## 2. Metric Integrity

- **Green:** Metrics prove better decisions. Baselines are real numbers.
- **Yellow:** Metrics are reasonable but baselines are "TBD" or "N/A".
- **Red:** Vanity metrics. Success without consequence.

## 3. Reality Awareness

- **Green:** Constraints and gaps are explicit. Non-goals are meaningful (tempting + dangerous).
- **Yellow:** Gaps acknowledged but buried. Non-goals are wishlists.
- **Red:** Idealized data. "We'll handle later."

## 4. Guardrail Strength

- **Green:** Defaults are opinionated. Bad states are blocked. Thresholds are numeric.
- **Yellow:** Guardrails exist but are vague ("should warn the user").
- **Red:** "Advanced users can configure." Validation deferred to docs.

## 5. Failure Coverage

- **Green:** Failure paths are named. Alerts map to actions.
- **Yellow:** Some failure paths covered. Alert actions are vague.
- **Red:** Happy path only. FYI alerts.

## 6. Operational Closure

- **Green:** Output feeds another system. Changes daily behavior.
- **Yellow:** Output is useful but doesn't explicitly connect to operations.
- **Red:** Results live in isolation. No follow-through path.

## 7. Integrity Test (Final Gate)

Ask: "Could this produce a result that looks correct but leads to a bad business decision?"

- **Green:** Risk is acknowledged and mitigated.
- **Yellow:** Risk is acknowledged but mitigation is vague.
- **Red:** Risk is implicit or ignored.

---

## Acceptance Bar

A PRD is **not acceptable** if:
- Any section is Red
- Guardrails are vague
- Failure modes are missing

A PRD is **strong** if:
- It makes misuse difficult
- It fails loudly and early
- It earns trust over time

## How to Use in Self-Review

1. Score each dimension
2. For any Yellow: attempt to fix. If you can't fix without more information, flag it as "needs additional research"
3. For any Red: must fix before presenting the draft
4. Do NOT show scores to the user. The self-review is silent — it improves the V0, the user sees only the result.
