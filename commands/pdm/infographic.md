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
2. Use `shot-scraper` to take a full-page retina screenshot. It auto-detects the page height (no guessing) and renders at 2x for crisp text:

```bash
shot-scraper "$HTML_PATH" -o "$SCREENSHOT_PATH" --width 1200 --retina
```

Pass the file path directly (not a `file://` URL). `--width 1200` matches the infographic's max-width; `--retina` renders at 2x device pixel ratio.

3. Save the screenshot to `/Users/bruce/notes/images/<slug>.png` where `<slug>` matches the HTML filename without the version suffix (e.g., `page-diagnostics-pipeline-xray.png`). If a file with that name already exists, overwrite it (the latest version is the one we want embedded).

## 3. File the HTML in the vault

1. Copy the final HTML to `/Users/bruce/notes/projects/<slug>.html`.
2. If there's an obvious parent project folder (e.g., `projects/diagnostics/`), file it there instead of `projects/` root.

## 4. Link into the weekly log

1. Find the current week's log in `log/2026/`.
2. Add a line in the Eng Log section under today's date entry:

```
- Created infographic: ![[<screenshot-slug>.png]] ([[<html-slug>|interactive version]])
```

The `![[...]]` embeds the screenshot inline. The `([[...|interactive version]])` links to the HTML for anyone who wants to open it.

If today's date section doesn't exist in the Eng Log, create it.

## Notes

- Follow the infographic skill's iteration process. This command adds the filing step after the skill finishes, not during.
- Canadian English spelling. No em-dashes in prose.
- Don't create a vault note with a H1 title (Obsidian renders the filename).
- The `images/` folder is for embedded images referenced by vault notes. Screenshots belong there.
