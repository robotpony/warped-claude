# Phase 3: Scope

Present MVP options, get a decision, expand into concrete scope bullets.

---

## Step 1: Generate MVP options

Read `references/mvp-options-format.md` for the gold standard format.

Using the research synthesis, generate 3-4 MVP options from narrowest to widest scope. Each option must be:
- Independently viable
- Clearly differentiated from the others
- Honest about what it doesn't do

Present to user with your recommendation on tradeoffs.

## Step 2: User selects scope

Wait for the user to pick an option (or propose a hybrid). One decision.

## Step 3: Expand into scope bullets

After the user picks an option, expand it into 5-8 specific scope bullets. These are the concrete boundaries for the PRD.

Example:
```markdown
## Scope (MVP 2 selected)

1. Build bucket ontology with 8 flat buckets, hardcoded at launch
2. Settings page for label-to-bucket mapping rules, placed below Retail Data
3. Default mapping template using existing MTA label facets
4. Per-adkey multiplier resolution in the MTA pipeline
5. Coverage report showing mapping rules and empty-bucket flags
6. Campaign count and spend per bucket (Stretch)
```

## Step 4: Confirm scope

Present the expanded bullets to the user: **"This is the scope I'll draft against. Anything to add or cut?"**

Wait for confirmation.

## Step 5: Record scope decision

Write the confirmed scope bullets to the **Decisions & Constraints** section of `research-synthesis.md`.

Format:
```markdown
### Scope Decision
**Selected:** MVP [N] — [name]

1. [Scope bullet]
2. [Scope bullet]
...

Confirmed by user [date].
```

Add `<!-- phase-complete -->` marker. Proceed to Phase 4.
