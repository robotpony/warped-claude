---
name: warped:banner-image
description: Generate a watercolour banner image prompt for a warpedvisions.org post
argument-hint: <post-path or idea>
---

# Generate a banner image prompt for: $ARGUMENTS

This command builds the prompt, generates the banner with `tools/banner.sh` in w42
(OpenAI's image API; key setup and `--check` are in w42's `tools/IMAGES.md`), and
runs a mock review cycle: every banner is a mock until the author approves it.

1. Determine the idea to illustrate:
   - If `$ARGUMENTS` is a path to a Hugo post or vault outline, read it and distill the
     core idea into one short phrase (not the full title, but the underlying concept the
     post argues, e.g. "load balancing across time" for a post about stunt turkeys and
     software releases)
   - Otherwise treat `$ARGUMENTS` as the idea phrase directly
   - If no args given, ask for the idea or post path
   - Confirm the phrase with the author before building the prompt if it's ambiguous

2. Pick a **simplified** subject for the idea. Details distract in these banners, so
   keep every object to its simplest recognizable form:
   - Name only the few parts that carry the idea (three to five), plainly: "bun,
     shredded lettuce, pickles, a thin griddled patty, bottom bun", not every layer with
     its variety, size, and treatment
   - Say "simplified" for each object ("a simplified notebook outline", "a simplified
     exploded burger"), and give layout in broad strokes (left, right, bleeding into)
   - No props, labels, leader lines, or anything that reads as fine detail
   - When asked for several themes, vary the subject and composition, not the detail

3. Write the prompt as **one paragraph** (OpenAI's image API has no negative prompt, so
   the no-text rule goes inside it), from this template:

   ```
   Watercolour painted technical banner, wide and letterboxed. <Simplified subject:
   a few named parts and where they sit>. Vibrant saturated washes, <colours, e.g.
   teal and blue into warm orange and red>, paper bleed and splatter. Loose and
   abstract, never sharp-edged. No frame, no border, no caption, no text, letters,
   or words anywhere.
   ```

   Example (the cookbook post's banner, 2026-10):

   > Watercolour painted technical banner, wide and letterboxed. On the left, a
   > simplified notebook outline; on the right, the same page bleeding into a simplified
   > exploded burger floating in layers (bun, shredded lettuce, pickles, a thin griddled
   > patty, bottom bun). Vibrant saturated washes, teal and blue into warm orange and
   > red, paper bleed and splatter. Loose and abstract, never sharp-edged. No frame, no
   > border, no caption, no text, letters, or words anywhere.

4. Present each prompt ready to copy-paste. `banner.sh` adds a no-text, no-logos,
   no-code guard to every prompt and asks for the banner's own wide shape (1536x560,
   trimmed to 1536x559), so the prompt needn't repeat either. For a post that wants
   lettering, logos, or sprites on purpose, pass `--allow-marks`. pbcopy the prompts
   under a `␣⌘ Copy · in w42` line. The visual target, to judge results against:
   - Vibrant, saturated watercolour washes with paper-bleed and splatter texture
   - Simple shapes in loose watercolour abstraction, never a literal, sharp-edged, or
     detailed illustration
   - Quiet space at the top, so it sits well above a title
   - No frame, border, caption, or embedded text of any kind

5. Generate and review (each run is a paid API call, so confirm before running):
   - Run `tools/banner.sh <slug> "<prompt>"` from w42. It saves the banner at
     `~/projects/sites/images.warpedvisions.org/YYYY/MM/<slug>.jpg` (the current month)
     and opens it for review
   - It's a mock: show it to the author, check it against the visual target above,
     and revise the prompt from their notes. Delete a rejected banner before the next
     run (the script won't overwrite one)
   - `tools/banner.sh --from <png> <slug>` recrops a kept full image without a new call

6. Once the author approves it, file it:
   - Add the prompt to the images repo's `prompts.md`, and commit there per its
     CLAUDE.md
   - The post references it in front matter: `banner:` with `src: YYYY/MM/<slug>.jpg`
     and `alt`
   - The alt text says the image is AI-generated and how, then what it shows:
     "AI-generated watercolour banner, made with OpenAI's image API from a prompt by
     Bruce: …" (site style guide, Other Rules)
   - The images repo must be deployed before the post goes live
