# Warped Claude

My Claude Code setup for product management, technical writing, and side projects. Commands, skills, and rules I use daily.

## How it's organized

```
commands/
  pdm/       Product management (Obsidian vault workflows)
  prd/       Product requirements (PRDs, teardowns, reviews)
  warped/    Blog writing (warpedperspective.com)
  mx/        Software engineering (general purpose)
  gdoc-pull.md   Standalone: pull a Google Doc into the vault
rules/
  product-writing-rules.md    PRD structure, formatting, anti-patterns
  blog-writing-rules.md       Voice, tone, style for blog posts
  blog-writing-reference.md   Themes, post types, patterns reference
  blog-writing-guidelines.md  Extended style guide (20+ years of patterns)
```

Commands are invoked as `/namespace:command` in Claude Code. All accept arguments for context (e.g., `/pdm:meeting-summary path/to/notes.md`).

## pdm: Product management

Commands for an Obsidian vault used as a PM work system: weekly logs, meeting notes, project briefs, task tracking. Most read from and write to the vault. Some pull data from ClickUp and Notion via MCP.

| Command | What it does |
|---------|-------------|
| `/pdm:new-week` | Monday ritual. Carries forward tasks, reviews the roadmap, proposes next week's tasks, creates the weekly log. |
| `/pdm:carry-forward` | Reads the current log and linked docs, separates TODOs into done/rolling/dead, groups by workstream. Run before `/weekly-log` when you want more control. |
| `/pdm:review-week` | End-of-week review. Collects metrics (TODOs completed, tickets created/completed, vault docs added, PRDs written) and writes a summary file alongside the weekly log. |
| `/pdm:standup-prep` | Generates a 3-8 bullet standup from the weekly log and recent ClickUp activity. Output only. |
| `/pdm:meeting-summary` | Summarizes a transcript or raw notes into a structured vault note with decisions, tasks, and topic index. |
| `/pdm:extract-tasks` | Pulls action items from any vault note, attributes owners, offers to insert into the weekly log. |
| `/pdm:plan-doc` | Creates a plan document and links it to the weekly log. |
| `/pdm:ooo-coverage` | Generates an OOO coverage doc from active projects and weekly logs. |
| `/pdm:priorities-sync` | Snapshots roadmap priorities from the Development Priorities doc. |
| `/pdm:roadmap-summary` | Summarizes roadmap sections by status and gaps. |
| `/pdm:release-notes` | Pulls completed tasks from ClickUp, formats for the weekly log's release notes section. |
| `/pdm:clickup-report` | Ticket creation/completion stats from ClickUp for a date range. |
| `/pdm:notion-extract` | Extracts a Notion page section into a local markdown doc. |
| `/pdm:gdoc-clean` | Cleans up a pasted Google Doc (fixes structure and formatting artefacts). |
| `/pdm:claude-tip` | Generates a short Claude tip for the team, suitable for a single Slack message. |

### Weekly workflow

**Monday:** `/pdm:new-week` creates the weekly log with carried-forward tasks, priority gaps, and proposed work. For more control, run `/pdm:carry-forward` first, then `/weekly-log`.

**During the week:** `/pdm:meeting-summary` after calls. `/pdm:extract-tasks` to pull action items into the log. `/pdm:standup-prep` before standups.

**Friday:** `/pdm:review-week` generates a summary doc with metrics and a qualitative review of priorities, gaps, and stale items.

## prd: Product requirements

Commands for writing, reviewing, and iterating on PRDs and design documents.

| Command | What it does |
|---------|-------------|
| `/prd-writer` | Full PRD authoring skill. Four mandatory phases (orient, research, scope, draft) with optional extensions. Tracks state with file markers. |
| `/prd:requirements` | Lighter alternative. Distills requirements from source documents (teardowns, summaries, transcripts) into a phased PRD. |
| `/prd:review` | Reviews a PRD for quality, coherence, and readiness. Accepts a Notion URL, vault path, or pasted content. Seven-point framework with delta tracking. |
| `/prd:teardown` | Reviews numbered screenshots of a prototype, produces a structured teardown. |
| `/prd:transcript-review` | Reviews a meeting transcript, produces a design/product review summary with decisions and actions. |
| `/prd:session-retrospective` | Reviews the current conversation, produces a structured retrospective focused on tooling and process improvements. |

### Typical flow

**From notes:** `/prd:requirements path/to/notes.md` to distill, then `/prd:review` to check quality.

**From scratch:** `/prd-writer` walks through the full process with checkpoints.

**After a design review:** `/prd:transcript-review` to summarize, then `/prd:requirements` to extract the structured spec.

## warped: Blog writing

Commands for drafting and publishing posts on warpedperspective.com. All follow the voice and style rules in `rules/blog-writing-rules.md`.

| Command | What it does |
|---------|-------------|
| `/warped:new-outline` | Asks clarifying questions about a post idea, produces a structured outline with headline options and opening hook. |
| `/warped:outline-draft` | Alias for `new-outline`. |
| `/warped:draft` | Writes a full first draft from an outline. Asks for missing personal anecdotes. Uses placeholders where author input is needed. |
| `/warped:review-draft` | Deep review: quality score, style adherence checklist, spelling/grammar, suggestions. |
| `/warped:post-checklist` | Pre-publish checklist: structure, content, style. |
| `/warped:socialize-post` | Generates platform-specific social blurbs (Mastodon, Bluesky, LinkedIn, Reddit). |

### Typical flow

Idea to publish: `/warped:new-outline` > iterate > `/warped:draft` > `/warped:review-draft` > revise > `/warped:post-checklist` > `/warped:socialize-post`

## mx: Software engineering

Lightweight development commands for side projects. Not domain-specific.

| Command | What it does |
|---------|-------------|
| `/mx:architect` | Analyze a problem, produce architecture artefacts. |
| `/mx:research` | Research components, tools, or approaches. |
| `/mx:ideate` | Brainstorm and explore a problem space. |
| `/mx:feature` | Implement a feature, update version/changelog/readme. |
| `/mx:bug` | Reproduce, root cause, fix, and regression check. |
| `/mx:nit` | Minor quality improvements: naming, style, small refactors. |
| `/mx:cleanup` | Review and clean up code, docs, unused files. |
| `/mx:iterate` | Review user feedback and iterate on work in progress. |
| `/mx:finish` | Finalize: version bump, build, changelog, readme, commit message. |

## Rules files

These define quality standards that the commands reference. They're loaded automatically by Claude Code when working in any project.

- **`rules/product-writing-rules.md`** — PRD structure, formatting, anti-patterns, review checklist. Influenced by Paul Graham, John Carmack, and RFC style. Used by prd: and pdm: commands.
- **`rules/blog-writing-rules.md`** — Voice, tone, style rules for the blog. Canadian English, minimal em-dashes, no corporate jargon. Used by warped: commands.
- **`rules/blog-writing-reference.md`** — Themes, post types (quick observation through personal experiment), and pattern reference.
- **`rules/blog-writing-guidelines.md`** — Extended style guide covering 20+ years of writing patterns, anti-patterns for AI-sounding prose, and voice calibration.

## Setup

These files live in `~/.claude/` and are picked up automatically by Claude Code. To use them:

1. Clone this repo into your `~/.claude/` directory (or symlink)
2. Commands appear as `/namespace:command` in Claude Code
3. Rules are loaded as global instructions for all projects

The pdm: commands expect an Obsidian vault and optionally use ClickUp and Notion MCP servers. The warped: and mx: commands work anywhere.
