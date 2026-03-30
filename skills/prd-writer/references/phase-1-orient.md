# Phase 1: Orient

Determine whether this is a fresh PRD or a continuation. Set up the workspace.

---

## Steps

### 1. Check for existing PRDs

Glob `prd-drafts/*/` in the current working directory (or wherever the user's PRD output lives) to find existing PRD folders.

If existing folders found:
- List them with what artifacts exist in each (research-synthesis.md, outline.md, prd.md, etc.)
- Check for `<!-- phase-complete -->` markers — a file without this marker is incomplete
- Ask the user: "Found these PRDs in progress. Want to continue one, or start fresh?"
- If continuing, ask: "Where do you want to pick up?" Don't infer the phase from files — the user knows where they left off.

If no existing folders found:
- Proceed to fresh PRD setup.

### 2. Fresh PRD setup

Ask the user: **"What's the PRD about?"**

One question. Wait for the answer.

From the user's response, generate a topic slug (lowercase, hyphens, no special chars). Create the output folder:

```
prd-drafts/{topic-slug}/
  research/
```

### 3. Confirm the research target

Ask: **"What codebase should I research?"**

Default: current working directory. The skill may be installed in an ops folder but researching a different repo. Don't assume.

### 4. Gather initial context

Ask the user for any of the following they can provide (one question at a time):
- Links to prior specs, design docs, or Figma files
- Names of related systems or features
- Any constraints or decisions already made
- Who the stakeholders are

Don't dump all these questions at once. Ask the most important one first, then follow up based on the answer.

### 5. Transition

When you have enough context to begin research, tell the user:

"Got it. Starting research — I'll explore the codebase and any docs you shared, then come back with what I found."

Proceed to Phase 2.
