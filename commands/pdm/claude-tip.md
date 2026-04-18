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

- **Slack formatting only.** Bold with asterisks, italic with underscores,
  backtick for commands. No markdown headers or code blocks.
- **Structure** (all parts required, in this order):
  1. **Header line:** `:party_cat: Bruce's Claude tip of the week :party_cat:`
  2. **One-sentence intro:** Direct statement of what the tip enables.
  3. **Numbered list (2-4 items):** Concrete actions the reader can take. Each
     item is one sentence. Line-break separated, not inline.
  4. **Payoff paragraph:** 1-2 sentences on why this compounds over time.
  5. **Personal practice note:** 1 sentence on your own cadence or experience.
  6. **"Here's what it looks like" close:** Name the specific command with
     backtick formatting, and optionally note where your tools live.
- **Tone: direct, peer-to-peer.** "Get more out of X by doing Y." Not "You
  should always" or "Best practice."
- **Canadian English spelling.**
- Do not include links.
- Do not write to any file. Output the tip directly in the conversation so the
  user can copy-paste into Slack.

## Example

Topic: session retrospectives

> :party_cat: Bruce's Claude tip of the week :party_cat:
>
> Get more out of your Claude sessions by running a retro at the end of a session:
>
> 1. Ask Claude to review the current session to recommend improvements for your local commands/skills and things the human can improve on
> 2. Also direct Claude to review any long-running, or inefficient commands, and consider different ways to approach.
> 3. Make improvements to your local skills and commands.
>
> The payoff is cumulative. Each retro surfaces a pattern you can bake into a command or habit, so the next session starts sharper. I find running a few retros a week for both good and bad sessions helpful to improve my patterns and tooling.
>
> In my sessions, the command looks like:
> `/prd:session-retrospective [additional instructions]`
> It suggests changes to my local `.claude` tools (which I keep up to date on Github).
