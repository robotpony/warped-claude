# Visual Design Reference — HTML Illustrations

This file is loaded by the visual-builder skill when building HTML output. It defines defaults for typography, color, spacing, and SVG diagramming. Apply these unless the user overrides.

---

## The readability test

Before considering any HTML visual done, open it at **100% browser zoom on a standard laptop screen**. Every word should be comfortable to read, not strained. If the user has to zoom to 125% or 150% to read it, the type is too small. This test trumps any other guidance below.

---

## Output format

Single self-contained HTML file:
- All CSS inline in `<style>` (no external stylesheets)
- Fonts loaded from Google Fonts via CDN
- No external JS dependencies
- Save to the `output` path with versioned filename: `<descriptive-name>-v1.html`, `v2`, etc.

---

## Typography

### Font pairing by visual type

| Visual type | Font pairing | Notes |
|------------|--------------|-------|
| Editorial illustration | **Playfair Display** (serif, italic for accents) + **DM Sans** (body, labels, data) | Newspaper/magazine aesthetic |
| System map | **Inter** or **DM Sans** throughout | Cleaner, more technical |
| Process / journey flow | **DM Sans** (body) + **DM Sans** (labels) | Same family, different weights |
| Comparison / matrix | **DM Sans** + **JetBrains Mono** for code/data cells | Mono for tabular data |
| Data chart | **DM Sans** (labels, titles) + **JetBrains Mono** (axis values, data labels) | Mono keeps numbers aligned |
| Annotated artifact | **DM Sans** + match the artifact's typography for callouts | Callouts should feel native to the artifact |
| Presentation slide | Same as above, but bumped sizes (see slide section below) | Readable from across a room |

Google Fonts import line (adjust families as needed):
```css
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,700;1,400&family=DM+Sans:wght@400;500;700&family=JetBrains+Mono:wght@400;500&display=swap');
```

**Fallback stacks** (always include system fallbacks in case Google Fonts CDN is slow or unavailable):
- Serif: `'Playfair Display', Georgia, 'Times New Roman', serif`
- Sans: `'DM Sans', system-ui, -apple-system, sans-serif`
- Mono: `'JetBrains Mono', 'SF Mono', 'Cascadia Code', monospace`

### Size scale (desktop)

| Role | Size | Notes |
|------|------|-------|
| Display / page title | **64-72px** | The h1, hero |
| Section head | **32-40px** | Major sections |
| Subhead / system name | **24-28px** | Card titles, sub-sections |
| Body / primary content | **18-20px** | Paragraphs, primary lists |
| Secondary / list items | **16-17px** | Sub-lists, captions inside cards |
| Captions / fine print | **14-15px** | Footers, byline-style text |
| Smallest allowed | **13px** | Never go below this |

**Floors (never violate):**
- Nothing below 13px.
- Letter-spaced caps labels (e.g., "EXTERNAL WITNESS · 01" treatment) need **14px+** — letter-spacing makes them feel smaller than the px value.
- Body content ≥ 18px on desktop, ≥ 14px on mobile.

### Hierarchy

At least 3 visual levels in every illustration:
1. **Headline** — large, serif or heavy sans
2. **Section label** — small caps sans, letter-spaced 0.18-0.24em, 14-16px
3. **Body** — regular sans, 18-20px

### Slide-specific sizing (presentation slide type)

For 16:9 slides intended to be read from across a room:
- Display: 80-120px
- Section head: 48-64px
- Body: 28-32px
- Smallest: 20px

---

## Color palette

Define roles, then fill with brand colors. Avoid pure black/white.

```css
:root {
  --paper: #F7F5F0;       /* Background — off-white, never #FFF */
  --paper-deep: #EFEBE2;  /* Slightly darker for inset areas */
  --ink: #1A1A2E;         /* Primary text — near-black with warmth */
  --ink-mid: #3D3D56;     /* Body text */
  --ink-dim: #6B6B82;     /* Secondary text */
  --ink-faint: #9B9BB0;   /* Tertiary text, captions */
  --rule: #D8D5CE;        /* Borders, dividers */
  --rule-soft: #E8E4DB;   /* Lighter dividers */

  /* Accent colors — assign to roles, not decorative.
     Up to 5 accents for multi-source/witness visuals.
     Tones are warm and muted; avoid saturated primaries. */
  --accent-1: #C24D2E;    /* Terracotta — external, ad platforms */
  --accent-2: #6B8F5C;    /* Sage — infrastructure, CDN */
  --accent-3: #3D4F8A;    /* Indigo — internal, product data */
  --accent-4: #8B6F4E;    /* Sienna — manual/custom sources */
  --accent-5: #7B5EA7;    /* Plum — future, planned */
}
```

**Brand colour override:** Start with the default palette above. Replace accent colours with brand colours when available. Keep paper/ink defaults unless the brand specifies explicit background/text colours.

**Rules:**
- Background is always `--paper` (off-white), never `#FFF`
- Primary text is `--ink` (warm near-black), never `#000`
- Use 2-5 accent colors, each tied to a category/role (not decorative)
- Tones should be warm and muted; saturated primaries look out of place against the paper/ink palette
- A color appearing in the diagram must match the color used in the legend/labels

---

## Spacing

Generous whitespace. Spacing scales with type size.

| If body is | Line-height | Section gap | Body padding |
|------------|-------------|-------------|--------------|
| 18-20px | 1.55-1.65 | ≥ 40px | ≥ 64px desktop, 40px mobile |

**Rule of thumb:** if it feels cramped, add more whitespace. The newspaper aesthetic depends on letting content breathe.

---

## SVG diagramming techniques

Use **inline SVG within HTML** for all custom diagrams. No Mermaid. No external diagram tools.

### Setup

```html
<svg viewBox="0 0 1200 140" preserveAspectRatio="xMidYMid meet" xmlns="http://www.w3.org/2000/svg">
  <defs>
    <!-- gradients here -->
  </defs>
  <!-- shapes here -->
</svg>
```

`viewBox` + `preserveAspectRatio="xMidYMid meet"` scales the SVG proportionally within its container. Use `none` only for full-width decorative elements (gradient beams, background bars) where distortion is acceptable.

### Technique 1: Gradient beams for flow

Conveys direction without arrows. Solid at source, faded at target.

```html
<defs>
  <linearGradient id="beamDown" x1="0" y1="0" x2="0" y2="1">
    <stop offset="0" stop-color="#D94F30" stop-opacity="0.20"/>
    <stop offset="1" stop-color="#D94F30" stop-opacity="0.04"/>
  </linearGradient>
</defs>
```

### Technique 2: Transparent polygons for many-to-one or one-to-many flow

A trapezoid that converges from a wide top to a narrow bottom. Wraps the gradient.

```html
<polygon points="151,0 231,0 555,140 525,140" fill="url(#beamDown)"/>
```

The 4 points: top-left, top-right, bottom-right, bottom-left of the trapezoid.

### Technique 3: Edge lines on the polygon

Thin colored lines along the polygon edges reinforce the beam shape without being heavy.

```html
<line x1="151" y1="0" x2="525" y2="140" stroke="#D94F30" stroke-width="1" stroke-opacity="0.55"/>
<line x1="231" y1="0" x2="555" y2="140" stroke="#D94F30" stroke-width="1" stroke-opacity="0.35"/>
```

Use slightly different opacities for the two edges (0.55 and 0.35) to create visual depth.

### Technique 4: Dashed strokes for future/tentative/optional elements

```html
<line x1="767" y1="0" x2="615" y2="140"
      stroke="#8B5CF6" stroke-width="1" stroke-opacity="0.45"
      stroke-dasharray="8,5"/>
```

The `8,5` is "8px dash, 5px gap" — readable but clearly distinct from solid strokes.

### Technique 5: Color-coded regions

Beams/backgrounds matching a category's accent color tie the diagram back to the legend or labels. The accent color in the SVG must match the accent color used in the corresponding text/card/label.

### Technique 6: Stem connectors

Thin vertical line connecting two stacked elements, with a small triangle arrow at the end:

```css
.stem {
  width: 1px;
  height: 56px;
  background: var(--rule);
  margin: 0 auto;
  position: relative;
}
.stem::after {
  content: '';
  position: absolute;
  bottom: -1px; left: -4px;
  width: 0; height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 6px solid var(--rule);
}
```

---

## Text patterns

### Em-dash definition lists

A core pattern for dense informational text. Bold label, em-dash, lighter description, all inline:

```html
<p><strong>Volume threshold</strong> — ignore low-traffic tails so the signal is real</p>
<p><strong>Exemptions</strong> — subtract documented "don't track" pages and scopes</p>
```

Use for methodology lists, diagnostic criteria, glossary-style explanations. Keeps the page compact without resorting to tables.

### Mixed serif/italic headlines

The headline mixes weights and styles for rhetorical emphasis. Factual parts in regular serif, rhetorical/emotional parts in italic serif:

```html
<h1>Five systems look at the same page. <em>Do they agree?</em></h1>
```

Use italic serif for the question, the provocation, or the "so what" in a headline. Never more than one italic phrase per headline.

### Section labels with middle-dot separators

Consistent labelling pattern: small-caps, letter-spaced, with category and number separated by a middle dot:

```css
.section-label {
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--ink-faint);
}
```

```html
<div class="section-label">EXTERNAL WITNESS · 01</div>
```

### Closing statement

Large mixed-weight serif line that lands the takeaway, followed by a small-caps tagline:

```html
<p class="closing-statement">
  <em>Four</em> external witnesses. <strong>One</strong> internal record.
</p>
<p class="closing-tagline">THE GAPS ARE THE AUDIT</p>
```

The closing statement is the single line someone remembers. It should be quotable and self-contained.

---

## Composition patterns

### Vertical narrative flow

The default composition for editorial illustrations. Content flows top-to-bottom as a story, not a dashboard. Each horizontal band is a distinct section with generous whitespace between bands:

1. **Header** — headline + subtitle + premise paragraph
2. **Sources/witnesses** — cards in a row, minimal styling
3. **Convergence** — SVG beams connecting sources to a central artifact
4. **Artifact** — the concrete thing being examined (browser mockup, document, diagram)
5. **Analysis** — what the audit/diagnostic proves, typically 2-3 columns of methodology
6. **Output** — results, classifications, rankings
7. **Closing** — large takeaway statement + tagline
8. **Footer** — source citation + category

The story builds: here's what we're looking at → here are the witnesses → here's the thing they're examining → here's what they prove → here's what comes out → here's the punchline.

### The "witnesses converging on artifact" pattern (editorial)
Multiple cards at the top, gradient beams converging down to a centered artifact (browser mock, document, etc.), with analysis below. Each witness gets its own beam colour matching its accent. The artifact is always concrete and specific (a real URL, a real page, a named thing). Good for: triangulation, multi-source diagnostics, comparison-against-truth visuals.

### Browser mockup as anchoring artifact

A minimal browser chrome (three dots + URL bar) containing a centred description of the page/entity being examined. This grounds abstract concepts in something tangible:

```css
.browser-mock {
  max-width: 520px;
  margin: 0 auto;
  border: 1px solid var(--rule);
  border-radius: 10px;
  overflow: hidden;
  background: white;
}
.browser-chrome {
  background: var(--paper-deep);
  padding: 10px 16px;
  display: flex;
  align-items: center;
  gap: 8px;
  border-bottom: 1px solid var(--rule-soft);
}
.browser-dots {
  display: flex; gap: 5px;
}
.browser-dots span {
  width: 10px; height: 10px;
  border-radius: 50%;
  background: var(--rule);
}
.browser-url {
  font-family: 'JetBrains Mono', monospace;
  font-size: 13px;
  color: var(--ink-dim);
  background: var(--paper);
  padding: 4px 12px;
  border-radius: 4px;
  flex: 1;
}
```

### The "stem and branch" pattern (system map)
Central element with branches radiating out. Good for: system architecture, dependency maps.

### The "swim lane" pattern (process flow)
Horizontal bands, each representing an actor or stage. Steps flow left-to-right within bands. Good for: workflows with multiple actors.

### The "matrix" pattern (comparison)
Grid with row labels and column labels. Cells contain values, icons, or marks. Good for: feature comparisons, before/after.

### The "centered artifact with margin annotations" pattern (annotated)
Artifact (screenshot, mockup) takes the center. Numbered callouts with leader lines extend to the margins. Good for: UX walkthroughs, design specs.

---

## Card styling

Cards should be minimal. The paper colour is the dominant background. Heavy coloured backgrounds on cards are a dashboard pattern, not an editorial one.

**Default card:**
```css
.card {
  background: var(--paper);
  border: 1px solid var(--rule-soft);
  padding: 24px;
}
```

**Inset card** (for secondary groupings):
```css
.card-inset {
  background: var(--paper-deep);
  border: 1px solid var(--rule-soft);
}
```

**Future/tentative card** (dashed border, not just SVG strokes):
```css
.card-future {
  background: var(--paper);
  border: 1px dashed var(--rule);
}
.card-future .badge {
  font-size: 11px;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  padding: 2px 8px;
  border-radius: 3px;
  background: var(--paper-deep);
  color: var(--ink-dim);
}
```

Colour should appear in text (section labels, accent type) and convergence beams, not as card backgrounds. When a card needs colour-coding, use a left border accent:

```css
.card-accented {
  border-left: 3px solid var(--accent-1);
}
```

---

## What to avoid

- Pure black (`#000`) text or pure white (`#FFF`) backgrounds — looks harsh and generic
- Body text smaller than 18px on desktop — strains the eye, requires zooming
- Mermaid or auto-generated diagrams — produces functional but generic output
- More than 4 accent colors in one visual — fragments attention
- Decorative use of color (e.g., a button is blue "because blue looks nice") — color must encode meaning
- Cramped spacing — when in doubt, add more whitespace
- Putting everything from the source material into the visual — the job is to distill, not transcribe
- Heavy coloured backgrounds on cards — use paper colour with subtle borders; colour belongs in text accents and convergence beams, not card fills
- Dashboard-style horizontal layouts with everything visible at once — editorial visuals tell a story vertically; each section earns its space
