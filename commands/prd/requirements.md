---
name: prd:requirements
description: Distill structured requirements from source documents (teardown, summary, transcript) into a phased PRD format
argument-hint: <path-to-source-docs-or-folder>
---

Distill structured requirements from source documents into a phased, design-ready PRD. This is a lightweight alternative to the full `/prd-writer` skill — use this when you already have review artifacts (transcript summary, teardown, screenshots) and need to turn them into actionable requirements quickly.

## Input

1. Parse $ARGUMENTS for paths. Accept one or more of:
   - A folder containing source docs (will read all `.md` files)
   - Individual file paths to a transcript summary, teardown, or notes
   - A screenshots folder (will read images for additional context)

2. If no paths given, check the current working directory for files matching common patterns: `*-summary.md`, `*-teardown.md`, `*-transcript*.md`. Ask the user to confirm.

3. Read all source documents. Build a working understanding of:
   - What's being built and why
   - Who the users are
   - What decisions have been made
   - What's unresolved
   - What's out of scope

## Analyze

4. Identify:
   - **Customer archetypes** — distinct user types with different needs or entry points
   - **Natural phases** — what depends on what, what can ship independently
   - **Decisions already made** — things settled in reviews that shouldn't be reopened
   - **Open questions** — unresolved debates, missing information, known unknowns
   - **Dependencies and constraints** — engineering blockers, capability gaps, external factors
   - **Out of scope items** — things explicitly deferred, with the reasoning

5. For each phase, extract:
   - User stories (first person, one sentence, specific)
   - Features with descriptions and Must/Should priorities
   - Constraints or blockers specific to that phase

## Write the requirements

6. Create the requirements file in the same directory as the source docs, named `{feature-slug}-requirements.md` or as specified by the user.

Use this structure:

```
# <Feature Name>: Requirements

<One-paragraph summary: what this is, what it replaces, how it surfaces.>

---

## The problem

<Why this needs to exist. What's broken or missing. Be specific.>

### <Customer archetypes if distinct types exist>

<Describe each type, their starting point, and what they need.>

### <Known dependencies or scope boundaries>

<Things that matter but aren't in scope for this tool.>

---

## Phases

| Phase | What ships | Depends on |
|-------|-----------|------------|

---

## Phase N: <Name>

<One sentence: what the user accomplishes in this phase.>

### User stories

- I <do thing> so that <outcome>.

### Features

| Feature | Description | Priority |
|---------|-------------|----------|

### Decisions already made

- <Decision with attribution and context>

### Open questions

- <Question with context on why it matters>

### Constraints

- <Blocker or limitation with attribution>

<repeat for each phase>

---

## Design Guide

If the source docs include a teardown or screenshots, add a design guide section:

### Views

| # | View | Purpose | User is thinking... | Screenshot ref |
|---|------|---------|---------------------|----------------|

For each view, include:
- An indented plain-text layout sketch (per the `product-writing` skill's diagram style)
- Design intent (why it's structured this way)
- Key interactions
- Watch-out-for notes

### Design principles

Distill 3-5 principles from review feedback. Format: principle statement, then one sentence of context.

---

## Out of scope (for now)

- **<Item>**: <Why it's deferred, who raised it, when to revisit>

---

## Success criteria

<Numbered list. Include at least one time/effort metric if the source docs mention current process pain.>
```

7. Present the requirements path and ask: "Want me to review these against the source docs for gaps?"

## Quality checks

Before presenting, verify:
- Every decision from the source docs appears in a "Decisions already made" section
- Every open question from the source docs appears in an "Open questions" section
- Every action item from the source docs maps to a feature or is listed as out of scope
- User stories are first person and specific (not "users can...")
- Feature priorities are Must or Should (no "Nice to have" — either it's in scope or it's out of scope)
- Success criteria include at least one measurable metric

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Tables for features, phases, and view inventories
- Indented plain-text diagrams for layout sketches (no box-drawing characters)
- Attribute decisions and quotes to specific people
- Bold key phrases for scannability, not full sentences
