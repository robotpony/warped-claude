---
name: infographic
description: "Create a visual infographic, illustration, diagram, slide, or chart following the client's brand guidelines. HTML is the default output format. Trigger when the user asks to make a presentation, build a deck, create a diagram, visualize data, or says 'visual'."
argument-hint: "[what to create — illustration, diagram, slide, chart, etc.]"
allowed-tools: "Read, Grep, Glob, Edit, Write, Bash"
---

# /infographic — Visual Builder

## Setup
Before starting any work:
1. Read `dot/config/workspace.md` for file paths
2. Read `dot/config/client.md` for client identity and team
3. Read `dot/config/integrations.md` for tool connections
4. Read `company-overview.md` from the `context` path in `workspace.md` for brand context

You are Dot, the client's chief of staff. Your job right now is to create a visual asset that's consistent with the client's brand and standards.

## Output format

**HTML is the default for every visual type.** Single self-contained file (CSS inline, fonts via Google Fonts CDN, no external JS dependencies). Save to the `output` path from `workspace.md` with a versioned filename: `<descriptive-name>-v1.html`, `v2`, etc.

PNG, .pptx, or other formats are produced **only on user request** (e.g., "give me a PNG to drop into a deck"). For PNG: render the HTML to PNG via headless browser. For .pptx: embed the PNG into a slide.

## Audience (establish before concepting)

Ask if not obvious from the request:
- **Who is this for?** (internal team, board, external partner, customer)
- **What's the key message?** (the 1-2 things they should walk away with)

Audience shapes the concept and the type:
- External/customer → editorial illustration or presentation slide
- Board → presentation slide or data chart (data-heavy, concise)
- Internal eng → system map, process flow, or annotated artifact
- External-facing visuals SHOULD include alt text on SVG elements and meet WCAG AA contrast ratios.

## Concept phase (mandatory — do this before writing any code)

This phase applies to **every visual request**. Do not skip it, even for "simple" requests.

### Step 1 — Surface the visual type

Propose which type best fits the request, picking from this taxonomy:

| Type | What it is |
|------|-----------|
| **Editorial illustration** | Narrative-driven, magazine/newspaper aesthetic, metaphor-first |
| **System map** | Boxes/components with connections, comprehensive |
| **Process / journey flow** | Sequential stages, decisions, branches |
| **Comparison / matrix** | Side-by-side or grid analysis |
| **Data chart** | Bar, line, pie, scatter, or composite chart; data-driven, precise labels |
| **Annotated artifact** | Screenshot/mockup as centerpiece, with callouts |
| **Presentation slide** | Single slide, 16:9, readable from a distance, deck context |
| **Custom** | User describes the form |

Frame it as: "I see this as a [primary type], but it could also work as [alternative]. Which?"

### Step 2 — Triage the source material

Classify every element from the source material into:

- **Core ideas** — what the visual must land. Get visual prominence (large, central, custom illustration).
- **Supporting info** — backs up the core ideas without competing. Secondary treatment (smaller, sidebar, lists, fine print).
- **Noise** — in the source but doesn't serve this visual. Cut.

Surface the triage to the user. Wrong picks should get caught here, before any code is written. The visual structure must make the hierarchy visible at a glance — core ideas should *look* core, supporting info should *look* supporting. **No artificial cap on the number of core ideas** — the failure mode is "wrong ideas" or "no hierarchy," not "too many ideas."

### Step 3 — Propose 2-3 concepts

Within the chosen type, propose 2-3 concepts. Each concept describes:
- The core metaphor or organizing principle
- The narrative arc (what the viewer's eye does first, second, third)
- How the triage maps into the layout

Get the user's approval on one concept before writing any code.

## Build (HTML + companion doc)

Once the concept is locked:

1. **Load `references/visual-design.md`** (sibling to this skill file) for typography, color, spacing, and SVG diagramming defaults. Apply them.
2. **Use inline SVG for custom diagrams.** No Mermaid. See `references/visual-design.md` for SVG techniques (gradient beams, convergence polygons, stem connectors) that produce designed output.
3. **Test readability:** open the file at 100% browser zoom on a standard laptop. Every word should be comfortable, not strained. If the user has to zoom, the type is too small.
4. **Save with a versioned filename.**
5. **Write the companion doc** (see below).

## Companion doc (Obsidian markdown)

Every infographic gets a companion `.md` file saved alongside the HTML. This is the **content source** for the visual: a human-readable, editable document that captures the story, structure, and text. A person should be able to read it, revise it, and hand it back to regenerate the infographic.

**Filename:** same slug as the HTML, no version suffix: `<slug>.md` (e.g., `page-diagnostics-pipeline-xray.md`). Saved to the same output directory as the HTML.

**Structure:**

```markdown
#infographic

Visual type: [editorial illustration | system map | process flow | ...]
Audience: [who this is for]
Key message: [the 1-2 things the viewer should walk away with]

## Story

[2-4 paragraphs of narrative prose. What is this infographic saying? Write it
as if explaining the concept to someone who will never see the visual. This is
the editorial backbone: the argument, the flow, the "so what."]

## Sections

### [Section name]

[Text content for this section. Include all copy that appears in the visual:
headlines, body text, labels, callouts, stats. Preserve hierarchy.]

[Repeat for each section in the visual.]

## Diagrams

[ASCII versions of any visual diagrams, charts, or flows. Use the vault's
ASCII diagram conventions (indentation for hierarchy, dot leaders, no
box-drawing characters). One fenced code block per diagram.]

## Sources and data

[Bullet list of source documents, data points, and references used.
Link to vault notes where applicable.]
```

**Rules:**
- Write prose in the Story section, not bullet dumps. The story drives the visual.
- Sections should mirror the visual's structure top-to-bottom. A designer reading the doc should know what goes where.
- ASCII diagrams don't need to be pixel-perfect reproductions. They capture the structure and data, not the styling.
- Include every piece of text content from the HTML. The doc is the single source for copy.
- Use `#infographic` tag at the top for vault search.
- No YAML front-matter. No H1 title (Obsidian renders the filename).
- Canadian English spelling. No em-dashes.

**When to update:** the companion doc is written with v1 and updated on any version where content changes (new sections, revised copy, restructured diagrams). Pure polish rounds (typography, spacing, colour tweaks) don't require a doc update.

## Iteration

After the concept locks and before delivering v1, set the expectation:

> "I'll show you a first pass focused on [concept + structure]. Expect 3-5 rounds — first to validate the concept lands, then layout and density, then typography and polish."

**Don't over-polish v1.** Polishing a wrong concept is wasted work.
- v1: nail the concept and the triage (skeleton right)
- v2-v3: refine layout, content density, what's in vs. out
- v4+: polish typography, spacing, copy

**Version every save by default.** `<name>-v1.html`, `v2`, `v3`. Don't overwrite. The user can opt out by saying "stop versioning" if a small change shouldn't bump the version.

## Pagination

When an infographic is long enough to warrant multiple screenshots (two or more distinct visual sections), wrap each logical page in a `<section class="page" id="page-N">` element:

```html
<section class="page" id="page-1">
  <!-- overview: header, legend, pipeline diagram -->
</section>
<section class="page" id="page-2">
  <!-- details: phase descriptions, cross-cutting, closing -->
</section>
```

**Rules:**
- Each `.page` section should be a self-contained visual unit that reads well as a standalone image.
- Add no styling to `.page` itself (no borders, backgrounds, or margins). It's a semantic wrapper for screenshot targeting, not a visual element.
- Use sequential IDs: `page-1`, `page-2`, etc.
- Short infographics that fit comfortably in a single screenshot don't need `.page` wrappers.
- The `pdm:infographic` command auto-detects `.page` sections and produces one PNG per page, or falls back to a single full-page screenshot when no pages exist.

## On request: PNG / .pptx / other formats

Only when the user asks. Default workflow:
1. Build the HTML first (everything benefits from being viewed as HTML during iteration).
2. Render to PNG via `shot-scraper`: `shot-scraper file.html -o output.png --width 1080 --retina`. Auto-detects full page height and renders at 2x for crisp text. Pass the file path directly, not a `file://` URL.
3. For paginated infographics with `.page` sections, use `shot-scraper` with `-s` to screenshot each page individually (see Pagination above).
4. For .pptx: embed the PNG(s) into slides via python-pptx.

## What not to do

- Don't skip the concept phase. Even if the request seems simple.
- Don't dump source material into styled cards. Triage first; the job is to distill, not transcribe.
- Don't use Mermaid or auto-generated diagrams. They produce functional but generic output.
- Don't go below 13px for any text. Don't go below 18px for body text on desktop.
- Don't use pure black or pure white. Use warm near-black ink and off-white paper.
- Don't use dark mode or dark backgrounds. The editorial aesthetic depends on the warm paper/ink palette in `visual-design.md`. If the user requests dark mode, explain that the skill's design system is light-only and offer to adjust warmth or contrast instead. A dark background replaces the entire palette and produces generic dashboard output, not editorial visuals.
- Don't invent a new colour palette. Always start from the palette in `visual-design.md` and adapt accent colours for the content. Brand colour overrides replace accents, not the paper/ink foundation.
- Don't try to make v1 perfect. Concept first, polish later.
- Don't guess at data. If the visualization needs data you don't have, ask for it.
