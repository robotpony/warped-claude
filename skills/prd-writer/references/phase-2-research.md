# Phase 2: Research

The heaviest phase. Research quality directly determines V0 quality. Every domain correction in the final version traces back to a research gap.

---

## Step 1: Launch parallel research agents

Use the **Agent tool** to launch 2-3 agents in parallel (single message, multiple Agent tool calls). Each agent runs in its own context — this keeps raw research out of the main conversation and lets threads run concurrently.

**Do not downgrade the model for research agents.** Research quality is the single biggest determinant of V0 quality. Let agents inherit the parent model.

Give each agent a detailed prompt: the PRD topic, what to explore, what to look for, and the output file path. The agent should write its findings directly to the output file.

**Agent A: Codebase exploration**
- Explore the target codebase for the systems this PRD touches
- Map current data models, APIs, state machines, and configuration
- Find platform-wide behaviors the feature must be consistent with
- Look for existing UX patterns that can be reused
- Output → `research/codebase.md`

**Agent B: External docs**
- Read any links the user provided (Notion specs, Google Sheets, Figma)
- Search for related documentation via available MCP tools (Glean, Slack, etc.)
- If the feature involves mapping/configuration, start building the actual table
- Output → `research/external-docs.md`

**Agent C: Prior specs** (if prior specs exist)
- Read all related prior specs
- Flag any requirement that the new PRD might contradict or change
- Note commitments made in prior specs that the new feature must honor
- Output → `research/prior-specs.md`

**Context management:** Each agent reads and summarizes in isolation. Only the distilled findings (via the output files) come back to the main context. Do NOT read raw external docs directly in the main conversation.

## Step 2: Synthesis

After all research agents complete, run a synthesis agent:
- Reads all raw outputs from `research/`
- Produces a structured synthesis with:
  - **Decisions & Constraints** section at the TOP (empty initially — user decisions go here)
  - Findings organized by topic
  - Confidence markers: "**Confirmed in code:** [path]" for verified facts, "**Uncertain:** [reason]" for unverified claims
  - Contradictions flagged explicitly
  - Concrete tables (mapping tables, state transitions) built from research, not described abstractly
- Save to `prd-drafts/{slug}/research-synthesis.md`

## Step 3: Contradiction detection (second pass)

Run a SECOND short agent that reads raw outputs + the finished synthesis:
- Ask: "What did the synthesis gloss over?"
- Synthesis naturally smooths contradictions — this pass catches them
- Append any findings to the synthesis under a "## Flagged by review" section

## Step 4: Present findings to user

Show the user a summary of what you found. Include:
- Key findings (3-5 bullets)
- Any contradictions or gaps
- Platform-wide behaviors that constrain the feature
- Concrete tables you built during research (the table IS the research artifact — show it)

Then ask follow-up questions. **One decision at a time.** Multi-option presentations count as one decision.

Example follow-ups:
- "We found conflicting info about X — which is current?"
- "The codebase does Y, but the spec says Z. Which should the PRD follow?"
- "What are the platform-wide behaviors this feature must be consistent with? (e.g., snapshotted or live?)"

## Step 5: Record user decisions

Every user decision gets appended to the **Decisions & Constraints** section at the top of `research-synthesis.md`.

Format:
```markdown
## Decisions & Constraints

1. **[Topic]:** [Decision]. — User confirmed [date]
2. **[Topic]:** [Decision]. — User confirmed [date]
```

These have highest priority during drafting — they override research findings.

## Step 6: Checkpoint

When follow-ups are resolved, present: **"Here's what I found. What did I miss?"**

Wait for the user's response. If they add new context, update the synthesis.

When the user is satisfied, add `<!-- phase-complete -->` to the bottom of `research-synthesis.md` and proceed to Phase 3.
