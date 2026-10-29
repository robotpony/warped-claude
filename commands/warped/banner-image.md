---
name: warped:banner-image
description: Generate a watercolour banner image prompt for a warpedvisions.org post
argument-hint: <post-path or idea>
---

# Generate a banner image prompt for: $ARGUMENTS

This session has no image-generation tool. This command builds the prompt for the
author to run by hand (usually the OpenAI Playground; `tools/banner.sh` in w42 calls
the API directly when it works), then handles filing the result once it exists.

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

4. Present each prompt ready to copy-paste, and pbcopy them under a `␣⌘ Copy · in w42`
   line. Note the visual target so the author can judge results against it:
   - Wide banner: ask for 1536x1024 (or the Playground's widest) with the subject in the
     middle band, since the banner is cropped to 1536x559
   - Vibrant, saturated watercolour washes with paper-bleed and splatter texture
   - Simple shapes in loose watercolour abstraction, never a literal, sharp-edged, or
     detailed illustration
   - No frame, border, caption, or embedded text of any kind

5. Once the author has a generated image file, file it:
   - Crop to 1536x559 and save as a JPEG (quality 85) in the `images.warpedvisions.org`
     repo at `~/projects/sites/images.warpedvisions.org/YYYY/MM/<slug>.jpg`, where
     `YYYY/MM` is the post's month. A 1536x1024 image from the API can go through
     `tools/banner.sh --from <png> <slug>`; anything else, `sips` directly
   - Add the prompt to that repo's `prompts.md`, and commit there per its CLAUDE.md
   - The Hugo post references it as the first line after front matter:
     `<img src="https://images.warpedvisions.org/YYYY/MM/<slug>.jpg" alt="<alt>" />`
   - The alt text says the image is AI-generated and how, then what it shows:
     "AI-generated watercolour banner, made with OpenAI's image API from a prompt by
     Bruce: …" (site style guide, Other Rules)
   - The images repo must be deployed before the post goes live
