---
name: prd:teardown
description: Review numbered screenshots of a prototype and produce a structured teardown document
argument-hint: <path-to-screenshots-folder> [optional: context-doc-path]
---

Review a folder of numbered screenshots and produce a structured teardown document describing each screen's key features, interactions, and design intent.

## Input

1. Parse $ARGUMENTS for:
   - **Screenshots folder** (required): path to a folder containing numbered screenshot files
   - **Context document** (optional): path to a summary, transcript review, or requirements doc to cross-reference

2. List the screenshot files, sorted by filename. Expect numbered prefixes (e.g., `001-`, `01-`, `1-`).

3. If a context document is provided, read it first to understand the feature, decisions, and open questions before reviewing screenshots.

## Analyze each screenshot

4. Read each screenshot image. For each one, document:
   - **Section header**: `## NNN — Title` using the number and a descriptive title derived from the filename and content
   - **Image reference**: relative path markdown image link
   - **Description**: What's on screen. Be specific about:
     - UI components and their state (expanded, selected, empty, populated)
     - Data shown (labels, values, counts, percentages)
     - Interactive elements (dropdowns, buttons, forms, toggles)
     - Information hierarchy (what's primary, secondary, contextual)
   - **Context from review** (if context doc provided): Connect what's on screen to decisions, feedback, or open questions from the review. Attribute to specific people where relevant.

5. After all screenshots, note any features or interactions visible in the prototype that weren't discussed in the context document. Call these out as observations, not recommendations.

## Write the teardown

6. Create the teardown file beside the screenshots folder (not inside it), named `{folder-name-without-screenshots}-teardown.md` or as specified by the user.

```
# <Feature Name>: Screenshot Teardown

<One-line description of what the prototype is and where the screenshots are.>

---

## NNN — <Descriptive Title>

![NNN](<relative-path-to-image>)

<Description of what's on screen, interactions, data shown.>

<Context from review if applicable.>

---

<repeat for each screenshot>
```

7. Present the teardown path and a count of screenshots reviewed.

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Use bold for UI element names on first mention within a section
- Reference context doc attributions naturally ("David flagged this in the review")
- Describe what you see, not what you think should change. Observations at the end, not inline opinions.
- If a screenshot is unclear or ambiguous, flag it with a question rather than guessing
