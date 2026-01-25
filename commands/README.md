# Warped Claude commands

Commands I use for writing and side projects. Nothing special here, just stuff I was repeating all the time. I've cut these instructions down over the last 6 months as Claude has better built-in instructions now.

## Writing Commands

| Command | Purpose |
| ------- | ------- |
| `/outline-draft` | Outline a new post through Q&A, following project style guidelines |
| `/new-outline` | Same as outline-draft |
| `/review-draft` | Deep review of a post: quality score, style adherence, spelling, suggestions |
| `/socialize-post` | Generate social media blurbs (Reddit, LinkedIn, Mastodon) in 3 variants each |
| `/research` | Research components, tools, or approaches with structured evaluation |

## Development Commands

| Command | Purpose |
| ------- | ------- |
| `/feature` | Implement a feature with clarifying questions, then update version/changelog/readme |
| `/bug` | Analyze and fix a bug, then update version/changelog/readme |
| `/cleanup` | Review and clean up code, docs, unused files, then update version/changelog/readme |
| `/finish` | Finalize changes: update version, build, changelog, readme, commit message |

## Usage

All commands accept `$ARGUMENTS` to specify context:

```bash
/review-draft the-new-post.md
/feature add dark mode toggle
/research state management libraries for React
```

