---
name: warped:review-draft
description: Deep review of a post draft with quality score, style checks, and improvement suggestions
argument-hint: <post-path>
---

# Review the post draft $ARGUMENTS

Re-read (checking for updates to the post) and think deeply about the content.

Find the theme first:

- State the post's theme (its thesis) in one sentence, quoting where the draft states it (usually the opening and the ending). Put it at the top of the review.
- Judge structure, proportion, and every ❌, suggestion, and question against that theme. Weight given to the theme is deliberate, not imbalance; a list or heading set is not a promise of equal coverage.
- Drop any point that doesn't serve the theme. A strong idea off the theme becomes an essay idea, not a restructure.
- Before reporting, check each flag against this site's review memories and drop the ones they rule out.

Then produce a review that considers the following:

- provide a quality score out of 10, based on similarity to Wired or other major tech publications and writers (like Paul Graham, John Carmack, Linus Torvalds, etc.)
- gauge adherence to the project style guideline using checkmarks ✅ and Xs ❌, including styles for headers, titles, and writing themes
- Use the `blog-writing` skill as the style guide (load it; its `reference.md` covers post types and themes)
- check spelling and grammar, and suggest fixes
- check for factual correctness

Also, provide suggestions and feedback to improve the post:

- ask questions for anything that is unclear
- make concrete suggestions for changes and improvements
- suggest opportunities for diagrams, example code, and anecdotes
- suggest missing concepts
- suggest narrative improvements

Also consider:

- ignore syntax specific to Hugo (example: `qgraph {class="warped-graph"}`)