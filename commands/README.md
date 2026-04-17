# Warped Claude commands

Custom commands and skills for writing, product management, and side projects.

Organized into four namespaces plus a few standalone commands. All commands accept `$ARGUMENTS` for context.

## pdm: Product management

Commands for an Obsidian vault used as a PM work system: weekly logs, meeting notes, project briefs, task tracking. Most commands read from and write to the vault.

| Command | What it does |
|---------|-------------|
| `/pdm:new-week` | Monday ritual. Carries forward tasks, reviews the roadmap priorities doc, proposes next week's tasks, and creates the weekly log. Replaces running carry-forward + weekly-log + manual editing separately. |
| `/pdm:carry-forward` | Reads the current log and all linked docs, separates TODOs into done/rolling/dead, groups by workstream, and writes a carry-forward section. Run before `/weekly-log` when you want more control than `/pdm:new-week`. |
| `/pdm:review-week` | End-of-week review. Collects metrics (TODOs completed, ClickUp tickets created/completed, vault docs added, PRDs written) and identifies priorities, gaps, stale items. Writes a `<date> - summary.md` alongside the weekly log. |
| `/pdm:standup-prep` | Generates a 3-8 bullet standup from the weekly log and recent ClickUp activity. Output only, doesn't write files. |
| `/pdm:meeting-summary` | Summarizes a transcript or raw notes into a structured vault note with decisions, tasks, and topic index. |
| `/pdm:extract-tasks` | Pulls action items from any vault note, attributes owners, and offers to insert them into the weekly log. |
| `/pdm:plan-doc` | Creates a plan document and links it to the weekly log. |
| `/pdm:ooo-coverage` | Generates an OOO coverage doc from active projects and weekly logs. |
| `/pdm:priorities-sync` | Snapshots the current roadmap priorities from the Development Priorities doc. |
| `/pdm:roadmap-summary` | Summarizes roadmap sections by status and gaps. |
| `/pdm:release-notes` | Pulls completed tasks from ClickUp and formats them for the weekly log's release notes section. |
| `/pdm:clickup-report` | Ticket creation/completion stats from ClickUp for a date range. |
| `/pdm:notion-extract` | Extracts a Notion page section into a local markdown doc. |
| `/pdm:gdoc-clean` | Cleans up a pasted Google Doc (fixes structure and formatting artefacts). |
| `/pdm:claude-tip` | Generates a short Claude tip for the PDM team, suitable for a single Slack message. Provide a topic or let it suggest one. |

### Weekly workflow

Monday: `/pdm:new-week` (or `/pdm:carry-forward` then `/weekly-log` for more control)

During the week: `/pdm:meeting-summary` after calls, `/pdm:extract-tasks` to pull action items into the log, `/pdm:standup-prep` before standups

Friday: `/pdm:review-week` to generate the summary doc with metrics

## prd: Product requirements

Commands for writing, reviewing, and iterating on PRDs and design documents.

| Command | What it does |
|---------|-------------|
| `/prd-writer` | Full PRD authoring skill. Four mandatory phases (orient, research, scope, draft) with three optional extensions. Tracks state with file markers. |
| `/prd:requirements` | Lighter than `/prd-writer`. Distills requirements from source documents (teardowns, summaries, transcripts) into a phased PRD format. |
| `/prd:review` | Reviews a PRD for quality, coherence, and readiness. Source-agnostic: accepts a Notion URL, vault file path, or pasted content. Seven-point framework with delta tracking across passes. |
| `/prd:teardown` | Reviews numbered screenshots of a prototype and produces a structured teardown document. |
| `/prd:transcript-review` | Reviews a meeting transcript and produces a design/product review summary with Q&A pairs, decisions, and actions. |
| `/prd:session-retrospective` | Reviews the current conversation and produces a structured retrospective focused on tooling and process improvements. |

### Typical flow

Start from notes: `/prd:requirements path/to/notes.md` to distill, then `/prd:review` to check quality.

Start from scratch: `/prd-writer` walks through the full process with checkpoints.

After a design review call: `/prd:transcript-review` to summarize, then `/prd:requirements` to pull out the structured spec.

## warped: Blog writing

Commands for drafting and publishing posts on warpedperspective.com. All follow the voice and style rules in `~/.claude/rules/blog-writing-rules.md`.

| Command | What it does |
|---------|-------------|
| `/warped:new-outline` | Asks clarifying questions about a post idea, then produces a structured outline with headline options and an opening hook. |
| `/warped:outline-draft` | Alias for `new-outline`. |
| `/warped:draft` | Writes a full first draft from an outline. Asks for missing personal anecdotes before writing. Uses placeholders where author input is needed. |
| `/warped:review-draft` | Deep review: quality score (compared to Wired, Paul Graham, Carmack), style adherence checklist, spelling/grammar, suggestions. |
| `/warped:post-checklist` | Fast pre-publish checklist: structure, content, style, SEO basics. |
| `/warped:socialize-post` | Generates three platform-specific social blurbs (Mastodon, Bluesky, LinkedIn, Reddit). |

### Typical flow

Idea to publish: `/warped:new-outline` > iterate > `/warped:draft` > `/warped:review-draft` > revise > `/warped:post-checklist` > `/warped:socialize-post`

## mx: Software engineering

Lightweight development commands. Not domain-specific.

| Command | What it does |
|---------|-------------|
| `/mx:architect` | Analyze a problem, produce architecture artefacts. |
| `/mx:research` | Research components, tools, or approaches with structured evaluation. |
| `/mx:ideate` | Brainstorm and explore a problem space. |
| `/mx:feature` | Implement a feature, update version/changelog/readme. |
| `/mx:bug` | Reproduce, root cause, fix, and regression check. |
| `/mx:nit` | Minor quality improvements: naming, style, small refactors. |
| `/mx:cleanup` | Review and clean up code, docs, unused files. |
| `/mx:iterate` | Review user feedback and iterate on work in progress. |
| `/mx:finish` | Finalize: version bump, build, changelog, readme, commit message. |

## Standalone commands

| Command | What it does |
|---------|-------------|
| `/weekly-log` | Creates a new weekly log from the vault template. Carries forward unchecked tasks. For a more thorough process, use `/pdm:new-week` instead. |
| `/todos` | Scans the vault for all open tasks, reports by priority, staleness, and location. |
| `/gdoc-pull` | Pulls a Google Doc into the vault. |

## Supporting rules files

These define quality standards that the commands reference:

- `~/.claude/rules/product-writing-rules.md` — PRD structure, formatting, anti-patterns. Used by prd: and pdm: commands.
- `~/.claude/rules/blog-writing-rules.md` — Voice, tone, style rules. Used by warped: commands.
- `~/.claude/rules/blog-writing-reference.md` — Themes, post types, and patterns reference.
