---
name: pdm:infographic
description: Create an infographic using the visual builder skill, screenshot it, file it in the vault, and link it into the weekly log
argument-hint: [what to visualize, e.g. "failure modes from the diagnostics PRD"]
---

Create an infographic and file it in the Obsidian vault at `/Users/bruce/notes/`.

## 0. Decide whether to screenshot

Screenshots are **on by default** but can be skipped. Treat the screenshot step as optional when any of the following apply:

- The user explicitly says "no screenshot," "skip screenshot," "html only," "don't screenshot," "no png," or similar.
- The user asks to "re-render" or "regenerate" an existing infographic *without* mentioning images, and the existing folder already has a PNG (the existing PNG is stale but the user may want to review the HTML before committing to a new render).
- The infographic is for an internal review pass that won't be linked into the weekly log yet.

When skipping screenshots: produce the HTML and the companion `.md`, file them per step 3, and either skip the weekly log link entirely or use the HTML-only link form in step 4. State up front in your reply that you're skipping screenshots so the user can override.

When in doubt, ask once: *"Skip the PNG screenshot, or generate it as usual?"*

## 1. Build the infographic

Run the `/infographic` skill with the user's request ($ARGUMENTS). Follow the full concept phase, triage, and build process defined in the skill. The skill saves HTML output to `~/.claude/output/` with versioned filenames.

Do not skip the concept phase. Do not shortcut straight to HTML.

## 2. Screenshot the final version (optional — see step 0)

Skip this entire step if step 0 determined screenshots are off. Otherwise:

After the user approves a version (or after v1 if they haven't asked for changes):

1. Get the final HTML path from the skill output.
2. Check whether the HTML contains paginated sections (`<section class="page">`).

### Paginated infographics (HTML has `.page` sections)

Use `shot-scraper` to screenshot each page section individually:

```bash
# For each page section found in the HTML:
shot-scraper "$HTML_PATH" -s "#page-1" -o "$IMG_DIR/<slug>-overview.png" --width 780 --retina --wait 1500
shot-scraper "$HTML_PATH" -s "#page-2" -o "$IMG_DIR/<slug>-details.png" --width 780 --retina --wait 1500
```

Name each PNG descriptively based on what the page contains (e.g., `-overview`, `-details`, `-timeline`). Don't use generic `-page-1`, `-page-2` suffixes. Read the page content to pick a meaningful name.

Pass the file path directly (not a `file://` URL). `--width 780` keeps the viewport narrower than the content's `max-width` so the content fills the frame without dead space on the right (renders at 1560px retina). Do not use `-p` (padding) flags; they add whitespace outside the content area. `--retina` renders at 2x device pixel ratio. `--wait 1500` ensures fonts load.

### Single-page infographics (no `.page` sections)

Use `shot-scraper` to take a full-page retina screenshot:

```bash
shot-scraper "$HTML_PATH" -o "$SCREENSHOT_PATH" --width 780 --retina --wait 1500
```

### Output location

Save all output (screenshots, HTML, companion doc) to the project subfolder under `infographics/` (see step 3).

## 3. File everything in the infographics folder

All infographic assets go in `/Users/bruce/notes/infographics/<project>/<week>/`, organized by project and week:

1. Determine the project name from context (e.g., `diagnostics`, `onboarding`, `search`). If unclear, ask.
2. Determine the current week folder name: use the Monday date of the current weekly log in ISO format (`YYYY-MM-DD`). For example, if the current weekly log is "April 20, 2026.md", the week folder is `2026-04-20`.
3. Create the path if it doesn't exist: `/Users/bruce/notes/infographics/<project>/<week>/`.
4. Copy to that folder:
   - The final HTML: `<slug>.html`
   - The companion doc: `<slug>.md`
   - Screenshot PNGs: `<slug>.png` or `<slug>-<suffix>.png` *(only if step 2 ran)*

If a file with that name already exists, overwrite it (the latest version is the one we want embedded). The companion doc is the editable content source for the infographic; it lives next to the HTML so a human can revise the story, sections, or diagrams and hand it back for regeneration.

When screenshots were skipped, do **not** delete an existing PNG in the folder — it represents an earlier render and may still be the version linked from the weekly log. Leave it in place; the user can ask for a re-screenshot later.

## 4. Link into the weekly log

Skip this step entirely when both of these are true: step 2 was skipped *and* the user gave no signal they want this filed in the weekly log this turn (e.g., a draft pass, an internal review, an iteration on an existing deck). When skipping, mention in your reply that the weekly log was not updated so the user can ask for it explicitly.

Otherwise:

1. Find the current week's log in `log/2026/`.
2. Add an entry in the Eng Log section under today's date.

**Do not embed screenshots inline** (`![[...]]`). Infographic PNGs are too large for useful inline display. Instead, link to the interactive HTML and to each PNG by name when PNGs exist.

If today already has an "Infographics:" list, append to it. Otherwise create one.

**With screenshots (default):**

For single-page infographics, add a numbered item:
```
Infographics:
1. Short description — [[<slug>.html|interactive]] · [[<slug>.png|PNG]]
```

For paginated infographics, list each page PNG:
```
Infographics:
1. Short description — [[<slug>.html|interactive]] · [[<slug>-overview.png|overview]] · [[<slug>-details.png|details]]
```

**Without screenshots (HTML-only):**

```
Infographics:
1. Short description — [[<slug>.html|interactive]]
```

If today's date section doesn't exist in the Eng Log, create it.

## Notes

- Follow the infographic skill's iteration process. This command adds the filing step after the skill finishes, not during.
- **Light mode only.** Always use the warm paper/ink palette from `visual-design.md` (off-white `#F7F5F0` background, near-black `#1A1A2E` text). Never use dark backgrounds, even in auto mode or when the concept phase is compressed. This is non-negotiable. Load `references/visual-design.md` and use its CSS custom properties before writing any HTML.
- Canadian English spelling. No em-dashes in prose.
- Don't create a vault note with a H1 title (Obsidian renders the filename).
- Infographic assets (HTML, PNG, companion docs) go in `infographics/<project>/`, not `images/` or `projects/`. The `images/` folder is for general vault screenshots only.

## Quote attribution style

When the infographic includes customer quotes, paraphrases, or any sourced statement, attribution lines MUST be:

- **Italic** (DM Sans italic, ~14px, `var(--ink-dim)`)
- **Prefaced with an em-dash** (e.g., `— Connor, Ridge advisory board`)
- On their own line below the quoted content, displayed as `block`
- Use a single class name (e.g., `.attribution`) consistently across quote cards, bright-spot cards, and any other attributed content — do not split attribution into separate "who" and "what" sub-elements

This is editorial typography for attribution and is the one place em-dashes are permitted in this command's output, even though prose elsewhere avoids them.

Example:

```html
<div class="quote">
  <p>"Interactive chat is novel friction. Scheduled reports fit existing behavior."</p>
  <span class="attribution">— Connor, Ridge advisory board, AI agent test</span>
</div>
```

```css
.attribution {
  display: block;
  font-family: 'DM Sans', sans-serif;
  font-style: italic;
  font-size: 14px;
  line-height: 1.45;
  color: var(--ink-dim);
}
```

Apply the same treatment to attribution-style lines that aren't strict quotes (e.g., "— David Herrmann, Austin, Stas, Victor (unanimous)" under a card). Consistency across the visual matters more than purity of the "quote" definition.

## Talk track in the companion doc

When the infographic is built to support a presentation, teach-back, briefing, or any spoken delivery, the companion `.md` MUST include a **Talk track** section. Add it immediately after the front-matter and before the `## Story` section.

Use this when the user mentions any of: "teach-back," "presentation," "deck," "briefing," "pitch," "onsite," "all-hands," "demo," "stakeholder review." If unsure, ask once: *"Will this be presented spoken? If yes, I'll add a talk track."*

Talk track structure:

```markdown
## Talk track

Targeted at [audience and format, e.g. "the Tuesday AM teach-back: 30 min present, 15 min Q&A"]. Times are guidance, not a script. The infographic is open on the screen while you present; each section below corresponds to a section of the visual.

### Open ([N min])
[2-4 sentences the presenter would actually say. First-person voice. Frame the why-now and the one shift in thinking.]

### [Section name matching the visual] ([N min])
[What to say walking through this section. Pull out the 1-3 evidence points worth landing verbally; trust the visual to carry the rest. Use direct quotes from the visual where they help.]

[Repeat one block per visual section.]

### Likely Q&A and pre-thought answers
[3-6 questions the audience is likely to ask, each with a 1-3 sentence answer. Cover the obvious ones (where this fits in the broader roadmap, what it costs to be wrong, who owns the next step) plus 1-2 specific to the topic.]
```

Rules for the talk track:

- Write in the presenter's first-person voice ("I went into H1 expecting…"). It's a script-shaped doc, not a memo.
- Section timings should sum to within ±10% of the available time. The presenter will adjust live; the doc gives them a budget.
- Don't restate the visual content verbatim — give the *delivery* of it. What does the presenter add that the slide doesn't?
- The Q&A section is where you bake in the user's likely points of contention from the source material. If the source flagged "this depends on X being true," the Q&A should address X.
- Talk track is updated when the underlying argument changes. Pure visual polish doesn't require a talk track update.
