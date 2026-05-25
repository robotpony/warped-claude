---
name: pdm:triage
description: Triage an inbound product request (CS thread, Slack ping, customer ask) and route it to the right vault location with ownership, existing-plans, and critical-read analysis
argument-hint: <paste Slack/CS thread, link, or describe the request>
---

Decide where an inbound product request belongs before writing it up. This command does the *thinking* step — ownership, existing coverage, detail quality, and a critical read — and then routes the output to the right folder or hands off to a more specific command.

Use this when something lands in your inbox (Slack, CS ping, customer call, internal idea) and you don't yet know if it's a bug, a QoL ask, a new candidate, already on the roadmap, or out of your area.

## Input

1. Accept `$ARGUMENTS` as the source. This may be:
   - A pasted Slack thread or CS message
   - A link (Slack permalink, Notion doc, gdoc)
   - A free-text description
   - A reference to a vault note (`[[note]]`)
2. If a vault note or external link is referenced, read/fetch it to get the underlying context. If a Slack permalink is given without thread content, ask the user to paste the thread (you cannot fetch Slack directly).
3. Extract: **who raised it**, **when**, **what they asked for**, and **what triggered the ask** (use case, customer, blocker).

## Separate distinct asks

4. If the thread or description contains more than one distinct request, list them and triage each separately. Don't merge unrelated asks into a single verdict — the meta-shops-shopify-order-tags doc is the model: a primary ask plus an adjacent ask, each routed differently.

## Gather context (always do these in parallel)

5. **Notion priorities** — fetch the Development Priorities database directly:
   - `mcp__claude_ai_Notion__notion-fetch` with `id: "collection://020011e4-1c05-42f7-8c5d-7ec61bd2adad"`
   - Scan all sections (App, Incrementality, Data Out, Data In, Pipeline Core, AI, Everything Else) for items that overlap the request. Note the Notion priority number, area, staffing status (🟢/🟡/🔴), and customer/MRR signals if shown.
6. **Vault search** — grep for related docs in `projects/`, `new-projects/`, `bugs/`, and `discussions/`. Look for prior write-ups of the same surface, the same customer, or the same theme.
7. **ClickUp** — query for related tickets. Per [[clickup-check-before-inferring]], don't infer absence from vault state.
   - If the MCP isn't authenticated, initiate `mcp__clickup__authenticate` and ask the user to authorize — don't silently skip.
   - Search the MTA folder lists (see [[reference_clickup_my_boards]]) first. NB3.0 Iterations (`901321226863`) is where most benchmark/profit-page tickets live.
   - If the request clearly sits outside MTA (e.g., Pipeline Core, Data In, AI), note that ClickUp wasn't searched and why.

## Determine ownership

8. Classify into one of:
   - **App (mine)** — Bruce's surface. Continue to verdict.
   - **Out of area** — another team owns it. Identify which team (Pipeline Core, Data In, Data Out, Incrementality, AI). The verdict should reflect this: it doesn't get a new-projects outline as if it were Bruce's to scope.
   - **Mixed** — part of the request is App, part is elsewhere. Split per the "separate distinct asks" step.

## Detail quality

9. Be honest about whether the source supports a decision:
   - Specific use case clear? Technical cause clear? Customer count / MRR signal? Alternative paths explored?
   - A single-customer one-liner is fine to file as a candidate, but say so explicitly. Don't inflate one-off asks into roadmap items.

## Critical read

10. Write 2–4 bullets giving the honest take. This is the section that distinguishes triage from intake. Cover at least:
    - Is the *real* problem what the requester named, or something adjacent (e.g., UI truthfulness vs. pipeline parity)?
    - Is this a single-customer signal or a recurring theme? If recurring, link the prior instances.
    - What's the cheapest correct action (often a copy fix, a pointer to existing work, or a deferral) vs. the expensive one being requested?

## Verdict and routing

11. Pick exactly one verdict per distinct ask and route accordingly:

| Verdict | Action |
|---------|--------|
| **Out of area** | Write triage doc to `new-projects/<slug>.md` with `Status: Triaged — out of area`. Add row to `new-projects/All unfiled projects.md` with the owning team in the Owner column. Model: `meta-shops-shopify-order-tags.md`. |
| **New candidate (mine)** | Write triage doc to `new-projects/<slug>.md` with `Status: Needs outline` (or `Triaged — candidate` if the triage doc itself stands in for the outline). Add row to `All unfiled projects.md`. Consider whether the lighter `/pdm:new-project` would be a better fit — if so, hand off. |
| **Fold into existing project** | Do not create a new top-level doc. Either: (a) append a "Related request" note at the bottom of the existing project doc with date, source, and one-line characterization, or (b) for projects under `projects/<name>/`, drop a short file in that folder (no triage scaffold — just a one-page request note). Report the path you updated. |
| **Bug** | Stop and invoke `/pdm:bug $ARGUMENTS` so the bug-report shape and weekly-log link happen. Don't duplicate. |
| **QoL feature (NB 3.5)** | Write to `projects/nb-3.5-requirements/<slug>.md` using the shape from `sales-chart-bar-toggle.md`: Problem, Ask, Acceptance criteria, Why it matters, Open questions, Related. Update the index table in `projects/nb-3.5-requirements/README.md` (most recent at top). |
| **No action** | Don't write a file. Report the verdict, what existing thing already covers it, and a one-line pointer to give the requester. |

## Write the triage doc

12. For verdicts that produce a triage doc (Out of area, New candidate), generate a kebab-case slug and write `new-projects/<slug>.md` with this structure:

```
**Source:** <Person> (<channel/context>, <date>)
**Source link:** <permalink if available>
**Status:** <Triaged — out of area | Triaged — candidate | Needs outline>
**Related:** [[<vault-doc>]], [[<other-doc>]]

## The request

<If one ask: 2–4 sentences. If multiple distinct asks, numbered list with each summarized in 1–3 sentences.>

## Does the source resolve it?

<Did CS/the thread close it? Partially? Is the requester unblocked?>

## Ownership

<App vs. which other area. Reference Notion priority numbers and staffing status. If split across areas, list each surface and its owner.>

## Existing plans

<Bulleted list: Notion priorities by number with links, vault docs by [[wikilink]], ClickUp tickets by ID. State explicitly when ClickUp was/wasn't searched and why.>

## Detail quality

<Honest assessment of whether there's enough signal to act. Bullets are fine.>

## Critical read

<2–4 bullets. The "would I bet on this?" take.>

## Next steps

<Concrete: file as candidate, raise in next planning slot, follow up with <person>, no action. Include any cheap-correct-action you identified (e.g., 30-min copy fix).>
```

## After filing

13. Report:
    - The verdict for each distinct ask.
    - The file path(s) written or updated.
    - Any handoff invoked (e.g., "routed to `/pdm:bug`").
    - Any ClickUp ticket IDs found, with back-link reminder if a `bugs/` or `projects/` doc was touched.
14. If the request came from a meeting, weekly log, or Slack thread that lives in the vault, suggest a back-link to the source.
15. Ask once: "Want me to add a follow-up task to this week's log?" — only when the verdict implies a future action (planning slot, follow-up with a person, copy fix to slot in).

## Formatting rules

- No em-dashes in prose (use commas or semicolons)
- Canadian English spelling (colour, behaviour, organize, optimize)
- Use `[[wikilinks]]` for internal vault references
- Lead with conclusions; the critical read is the point of the doc, not an afterthought
- Keep triage docs focused. If the verdict turns into real scoping work, that's a different command (`/pdm:new-project`, `mx:architect`, or a project brief)
- Do not start the doc with an H1; the filename is the title
