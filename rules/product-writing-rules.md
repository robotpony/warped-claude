# Product Management Writing Rules

Rules for product briefs, analyses, RFCs, and professional documents. Shares language foundations with `blog-writing-rules.md` but optimized for clarity and decision-making.

## Core Principles

Write like you're explaining something to a smart colleague who has five minutes. Every sentence should earn its place.

**Influences:**
- Paul Graham: Short sentences, concrete examples, no jargon
- John Carmack: Technical precision, honest trade-offs, clear reasoning
- RFCs: Structured sections, precise requirements, edge case consideration

## Voice & Tone

- Direct and precise, not corporate
- State conclusions first, then support them
- Acknowledge uncertainty explicitly ("We don't know X yet")
- Name trade-offs; don't hide them in caveats
- Write to inform decisions, not to cover yourself

## Structure Principles

### Lead with the point

Bad: "After extensive research and consideration of multiple factors, we've determined that..."
Good: "We should build X. Here's why."

### One idea per paragraph

Each paragraph should have a single purpose. If you're covering multiple points, use a list or separate paragraphs.

### Use headers as navigation

Headers should let someone skim the document and understand the structure. A reader should be able to find what they need without reading everything.

### Front-load summaries

Every document over one page needs an executive summary at the top. Write it last, but put it first.

## Formatting Rules

### Bold and emphasis
- Use bold sparingly for scanability, not decoration
- If everything is emphasized, nothing is
- Maximum 2-3 bold items per page
- Never bold full sentences

### Lists
- Use numbered lists for sequences or priorities
- Use bullet lists for unordered items
- Keep list items parallel in structure
- One sentence per item unless complexity requires more

### Em-dashes
- Avoid entirely in professional documents
- Use commas, semicolons, or separate sentences instead
- Exception: occasional use for aside in informal updates

### Tables
- Use for comparisons and structured data
- Include clear column headers
- Keep cells brief; link to details

## Language Rules

Inherited from blog writing standards:

### Spelling (Canadian English hybrid)
- British: colour, favour, behaviour, neighbour, flavour, grey
- American: -ize endings (realize, organize, optimize), center, meter, liter

### Units
Metric with optional conversions: "20°C (68°F)", "5 km (3 miles)"

### Requirement language (RFC 2119 style)
- MUST / MUST NOT: Absolute requirements
- SHOULD / SHOULD NOT: Strong recommendations, exceptions need justification
- MAY: Optional, truly discretionary

Use these terms precisely and consistently. If you write "must," mean it.

## Document Types

### Product Brief / PRD

**Required sections:**
1. Summary (2-3 sentences: what and why)
2. Problem (what's broken or missing)
3. Solution (what we're building)
4. Success criteria (how we measure success)
5. Scope (what's in, what's explicitly out)
6. Open questions (what we still need to resolve)

**Optional sections:**
- User stories or scenarios
- Technical constraints
- Dependencies
- Rollout plan
- Risks and mitigations

### Analysis Document

**Structure:**
1. Question (what we're trying to answer)
2. Summary (the answer, upfront)
3. Methodology (how we got the answer)
4. Findings (the evidence)
5. Limitations (what could be wrong)
6. Recommendations (what to do next)

### Decision Document

**Structure:**
1. Decision (state it clearly)
2. Context (why this decision was needed)
3. Options considered (brief descriptions)
4. Rationale (why this option)
5. Trade-offs accepted (what we're giving up)
6. Reversibility (how hard to change later)

### RFC / Technical Proposal

**Structure:**
1. Abstract (one paragraph summary)
2. Motivation (why this matters)
3. Proposal (the actual proposal)
4. Specification (precise details)
5. Alternatives considered
6. Security / Risk considerations
7. Implementation notes
8. Open questions

### Status Update

**Structure:**
1. Summary (one sentence: are we on track?)
2. Progress (what's done)
3. Blockers (what's stuck)
4. Next steps (what's coming)
5. Decisions needed (if any)

Keep updates under one page. Link to details.

## Anti-Patterns

### Corporate jargon
Never use:
- "leverage," "synergize," "alignment," "stakeholder buy-in"
- "move the needle," "circle back," "take offline"
- "best practices," "learnings," "actionable insights"

Use plain words: "use," "work together," "agreement," "ask them."

### Hedge stacking
Bad: "We might potentially consider possibly exploring..."
Good: "We're exploring X. We'll decide by [date]."

### Passive responsibility
Bad: "It was decided that..."
Good: "We decided..." or "[Name] decided..."

### Vague timelines
Bad: "In the near future," "soon," "eventually"
Good: "By Q2," "within two weeks," "no timeline yet"

### Scope creep in writing
If a section grows beyond its purpose, it needs its own document. Link to it.

### Bullet point overload
If your document is 90% bullets, you're not explaining, you're listing. Use prose to connect ideas.

## Precision Practices

### Define terms
If a term could mean multiple things, define it at first use. Keep a glossary for recurring terms.

### Quantify when possible
Bad: "Significant performance improvement"
Good: "Response time dropped from 2.3s to 0.4s"

### Specify scope
Bad: "Users want this feature"
Good: "In 23 interviews, 18 users mentioned this need unprompted"

### Explicit assumptions
State assumptions clearly. If a recommendation depends on something being true, say so.

### Name the unknowns
Every document should acknowledge what you don't know. This builds trust and prevents bad decisions.

## Review Checklist

Before sending any document:

1. Can someone understand the main point from the first paragraph?
2. Are there any sections that could be cut without losing value?
3. Are the trade-offs named explicitly?
4. Are timelines and ownership specific?
5. Are there undefined terms that could confuse readers?
6. Do we state what we don't know?
7. Is there a clear next action or decision?
