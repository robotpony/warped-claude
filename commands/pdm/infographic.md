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
shot-scraper "$HTML_PATH" -s "#page-1" -o "$IMG_DIR/<slug>-overview.png" --width 1080 --retina --wait 1500 -p 48
shot-scraper "$HTML_PATH" -s "#page-2" -o "$IMG_DIR/<slug>-details.png" --width 1080 --retina --wait 1500 -p 24
```

Name each PNG descriptively based on what the page contains (e.g., `-overview`, `-details`, `-timeline`). Don't use generic `-page-1`, `-page-2` suffixes. Read the page content to pick a meaningful name.

Pass the file path directly (not a `file://` URL). `--width 1080` gives a crisp 2160px retina image; `--retina` renders at 2x device pixel ratio. `-p 48` adds padding around the selector match (use 48 for the first page, 24 for subsequent pages). `--wait 1500` ensures fonts load.

### Single-page infographics (no `.page` sections)

Use `shot-scraper` to take a full-page retina screenshot:

```bash
shot-scraper "$HTML_PATH" -o "$SCREENSHOT_PATH" --width 1080 --retina
```

### Output location

Save screenshots to `/Users/bruce/notes/images/<slug>[-suffix].png` where `<slug>` matches the HTML filename without the version suffix (e.g., `page-diagnostics-pipeline-xray.png` or `nb-data-health-roadmap-overview.png`). If a file with that name already exists, overwrite it (the latest version is the one we want embedded).

## 3. File the HTML and companion doc in the vault

1. Copy the final HTML to `/Users/bruce/notes/projects/<slug>.html`.
2. Copy the companion doc to `/Users/bruce/notes/projects/<slug>.md`.
3. If there's an obvious parent project folder (e.g., `projects/diagnostics/`), file both there instead of `projects/` root.

The companion doc is the editable content source for the infographic. It lives next to the HTML so a human can revise the story, sections, or diagrams and hand it back for regeneration.

## 4. Link into the weekly log

1. Find the current week's log in `log/2026/`.
2. Add a line in the Eng Log section under today's date entry.

For single-page infographics:
```
- Created infographic: ![[<slug>.png]] ([[<slug>.html|interactive version]])
```

For paginated infographics, embed each page image in sequence:
```
- Created infographic: ([[<slug>.html|interactive version]])
  ![[<slug>-overview.png]]
  ![[<slug>-details.png]]
```

The `![[...]]` embeds the screenshot inline. The `([[...|interactive version]])` links to the HTML for anyone who wants to open it.

If today's date section doesn't exist in the Eng Log, create it.

## Notes

- Follow the infographic skill's iteration process. This command adds the filing step after the skill finishes, not during.
- **Light mode only.** Always use the warm paper/ink palette from `visual-design.md` (off-white `#F7F5F0` background, near-black `#1A1A2E` text). Never use dark backgrounds, even in auto mode or when the concept phase is compressed. This is non-negotiable. Load `references/visual-design.md` and use its CSS custom properties before writing any HTML.
- Canadian English spelling. No em-dashes in prose.
- Don't create a vault note with a H1 title (Obsidian renders the filename).
- The `images/` folder is for embedded images referenced by vault notes. Screenshots belong there.
