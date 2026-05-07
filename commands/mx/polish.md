---
name: mx:polish
description: Make a visual/UX polish improvement with a patch version bump
argument-hint: <polish description>
---

# Polish: $ARGUMENTS.

This is a polish task, not a bug fix. The thing already works — make it feel better. Sparkle, considered detail, micro-interactions, spacing rhythm, typography hierarchy, hover/focus/active states, motion easing, copy tone, light/dark parity.

Follow these steps:

1. **Look and feel first.** Use AskUserQuestion liberally to understand the *intent*, not just the change. Ask about mood, weight, hierarchy, what should lead the eye, what should recede. When the change is visual, ask for a screenshot if one wasn't provided. When the user describes a feeling ("too heavy", "feels off"), ask what they'd compare it to or what good would look like — don't guess.

2. **Suggest standards.** If a relevant platform convention applies (Obsidian sidebar patterns, common a11y contrast targets, established motion durations like 120–200ms for UI feedback, hover/focus state norms), name it as an option. Don't lecture — offer it as one path the user can pick or override.

3. **Propose, don't just implement.** For non-trivial polish, give the user 2–3 concrete options before coding (e.g., "subtle / standard / pronounced"). Use AskUserQuestion previews when the choice is visual.

4. **Sweat the small stuff.** Check the change at rest, on hover, on focus, when disabled, when in motion (if relevant). Check both light and dark themes if the codebase supports them. Look for what *else* in the surrounding view would benefit from the same treatment for consistency — surface those as optional follow-ups, don't silently expand scope.

5. **Version + docs.** Bump the patch version (last digit only). Update the changelog under a "Changed — Polish" or similar heading. Update README only if user-visible behaviour or vocabulary changed. Provide a commit message that signals polish, not a fix or new feature.

   Suggested commit prefix: `polish:` — e.g., `polish: tighter focus card spacing` or `polish: warmer empty-state copy`.
