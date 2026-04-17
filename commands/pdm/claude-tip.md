---
name: pdm:claude-tip
description: Write a short Claude tip for the PDM team, suitable for a single Slack message
argument-hint: <topic, e.g. "session retrospectives" or "meeting summaries">
---

Write a short tip about using Claude effectively for product management work.
The audience is PMs and managers who are newer to Claude Code.

## Input

If $ARGUMENTS contains a topic, write a tip about that topic.

If no topic is provided, suggest 3-4 topics drawn from common PM workflows
(weekly planning, meeting notes, PRD writing, task management, session review,
ClickUp integration, etc.) and let the user pick one.

## Output rules

The tip is a single Slack message. Follow these constraints exactly:

- **3-7 sentences.** No more.
- **Plain text only.** No markdown headers, no code blocks, no bullet lists.
  Slack formatting (bold with asterisks, italic with underscores) is fine.
- **Name the specific command or technique** so the reader can try it immediately.
- **Open with a direct statement** of what the tip enables, then name the
  command or technique. Not the other way around.
- **Use inline numbered lists** (e.g., "It does three things: 1) X, 2) Y, and
  3) Z") to pack specifics into a short message. Prefer this over prose when
  listing what a command produces.
- **Close with why it compounds** or what changes when you do this regularly.
- **Tone: direct, peer-to-peer.** "Get more out of X by doing Y" or "Try Z
  after your next session." Not "You should always" or "Best practice."
- **Canadian English spelling.**
- Do not include links.
- Do not write to any file. Output the tip directly in the conversation.

## Example

Topic: session retrospectives

> Get more out of your Claude sessions by running a retro at the end. After a
> longer working session, try `/prd:session-retrospective`. It reviews what you
> just did together and produces three things: 1) where time went to mechanics
> instead of substance, 2) where existing commands could have saved steps, and
> 3) where you could front-load context to cut round trips. The payoff is
> cumulative. Each retro surfaces a pattern you can bake into a command or habit,
> so the next session starts sharper.
