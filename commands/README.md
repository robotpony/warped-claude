# Warped Claude commands

Commands I use for writing and side projects. Nothing special here, just stuff I was repeating all the time.

## Writing Commands

| Command | Purpose |
| ------- | ------- |
| `/outline-draft` | Outline a new post through Q&A, following project style guidelines |
| `/new-outline` | Same as outline-draft |
| `/draft` | Write a first draft from an outline or notes |
| `/review-draft` | Deep review of a post: quality score, style adherence, spelling, suggestions |
| `/post-checklist` | Fast pre-publish checklist against blog-writing-rules.md |
| `/socialize-post` | Generate social blurbs (Reddit, LinkedIn, Mastodon, Bluesky) in 3 variants each |
| `/research` | Research components, tools, or approaches with structured evaluation |

## Development Commands

| Command | Purpose |
| ------- | ------- |
| `/feature` | Implement a feature with clarifying questions, then update version/changelog/readme |
| `/bug` | Reproduce, root cause, fix, and regression check a bug |
| `/cleanup` | Review and clean up code, docs, unused files, then update version/changelog/readme |
| `/finish` | Finalize changes: update version, build, changelog, readme, commit message |
| `/nit` | Minor quality improvements: naming, style, small refactors |

## Design & Thinking Commands

| Command | Purpose |
| ------- | ------- |
| `/architect` | Analyze a problem and produce architecture artifacts (ARCHITECTURE.md, DESIGN.md, etc.) |
| `/ideate` | Brainstorm and explore a problem space, produce IDEAS.md or OUTLINE.md |
| `/iterate` | Review work-in-progress, agree on direction changes, update plans |

## Usage

All commands accept `$ARGUMENTS` to specify context:

```bash
/review-draft the-new-post.md
/feature add dark mode toggle
/research state management libraries for React
/bug login fails when email has a + sign
```
