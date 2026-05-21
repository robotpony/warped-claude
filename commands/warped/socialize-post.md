---
name: warped:socialize-post
description: Generate platform-specific social media blurbs for a published post
argument-hint: <post-path>
---

# Create social blurbs for post: $ARGUMENTS

Read the post and produce 3 variants for each platform below. Follow the `blog-writing` skill (load it first) for voice and tone.

## Platforms

| Platform | Limit | Tone |
|----------|-------|------|
| **Mastodon** | 500 chars | Conversational, link at end |
| **Bluesky** | 300 chars | Punchy, one strong hook |
| **LinkedIn** | 1300 chars | Professional but still direct; no corporate jargon |
| **Reddit** | No limit | Specific to subreddit context; lead with value, not self-promotion |

## For each platform

- Variant 1: Lead with the core insight
- Variant 2: Lead with a question or provocation
- Variant 3: Lead with a personal angle or anecdote hook

## Rules

- No "check out my post" or "I wrote about X" openers
- No excessive hashtags (max 2 on Mastodon/Bluesky, none on Reddit)
- Match the voice: conversational, direct, no jargon
- Flag which variant you'd recommend and why
