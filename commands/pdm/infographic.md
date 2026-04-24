---
name: pdm:infographic
description: Create an infographic using the visual builder skill, screenshot it, file it in the vault, and link it into the weekly log
argument-hint: [what to visualize, e.g. "failure modes from the diagnostics PRD"]
---

Create an infographic and file it in the Obsidian vault at `/Users/bruce/notes/`.

## 1. Build the infographic

Run the `/infographic` skill with the user's request ($ARGUMENTS). Follow the full concept phase, triage, and build process defined in the skill. The skill saves HTML output to `~/.claude/output/` with versioned filenames.

Do not skip the concept phase. Do not shortcut straight to HTML.

## 2. Screenshot the final version

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
   - Screenshot PNGs: `<slug>.png` or `<slug>-<suffix>.png`

If a file with that name already exists, overwrite it (the latest version is the one we want embedded). The companion doc is the editable content source for the infographic; it lives next to the HTML so a human can revise the story, sections, or diagrams and hand it back for regeneration.

## 4. Link into the weekly log

1. Find the current week's log in `log/2026/`.
2. Add an entry in the Eng Log section under today's date.

**Do not embed screenshots inline** (`![[...]]`). Infographic PNGs are too large for useful inline display. Instead, link to both the interactive HTML and each PNG by name.

If today already has an "Infographics:" list, append to it. Otherwise create one.

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

If today's date section doesn't exist in the Eng Log, create it.

## Notes

- Follow the infographic skill's iteration process. This command adds the filing step after the skill finishes, not during.
- **Light mode only.** Always use the warm paper/ink palette from `visual-design.md` (off-white `#F7F5F0` background, near-black `#1A1A2E` text). Never use dark backgrounds, even in auto mode or when the concept phase is compressed. This is non-negotiable. Load `references/visual-design.md` and use its CSS custom properties before writing any HTML.
- Canadian English spelling. No em-dashes in prose.
- Don't create a vault note with a H1 title (Obsidian renders the filename).
- Infographic assets (HTML, PNG, companion docs) go in `infographics/<project>/`, not `images/` or `projects/`. The `images/` folder is for general vault screenshots only.
