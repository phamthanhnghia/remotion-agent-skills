# Intent interview — Remotion edition

Fresh creation only — the SKILL.md state table already decides whether this layer runs at all (edits, project operations, and briefed projects never enter it). One conversation turns "make me a video" into a confirmed brief — the route, the must-have answers, the run shape — handed to whichever workflow executes and made durable as `BRIEF.md`.

## The eight steps

**1 — Memory before questions.** Two reads, both mandatory, before anything is asked:

- **Remembered defaults.** Check the project root for a `.remotion-prefs.json` or equivalent preferences file. If none, offer fast defaults from `references/house-style.md` in `remotion-creative`.
- **Recipes.** If the user names a recipe or says "like last time," offer to adopt it before other brief questions — say why it matches and what adopting saves.

**2 — Triage the input.** What is the video about? Is the request **formed** (message, material, and occasion readable) or **unformed** (subject with no take on it)? A URL or document carries its own thesis but not the telling — "make a video about this URL" is formed about facts and unformed about the telling. An unformed request goes through the pitch round after routing.

**3 — Pick the route** (the route table in SKILL.md), then read `routes/<workflow>.md`. Its Interview section lists the must-have questions to ask now.

**4 — The pitch round** — unformed requests only. Sample five concepts along five genuinely different paths, at least two from the distribution's tail, and present them all before recommending one. Each pitch names the Remotion capability or two it rides.

**5 — The route's must-haves.** One question per field, recommended option first. Skip a question only when the request already answered it.

**6 — The two run-shape questions** — after the must-haves:

- **(a) Storyboard?** Review the plan pass-by-pass on a storyboard, or skip and get one finished video from the confirmed brief.
- **(b) Automation or companion?** **Automation** — the matched workflow's pipeline executes the brief end to end. **Companion** — build it together in `/remotion-general` with every Remotion capability on the table.

These two are **orthogonal — never merge them into one menu.** All four `flow` × `storyboard` combinations are valid.

**7 — Nice-to-have: recommend, then show.** One or two capability rows from `capability-menu.md` that the confirmed concept specifically calls for. Plus:

- Anything here you want?
- Any material of your own (images, clips, logos, data) the video should carry?
- Design spec: use an existing spec, pick a visual style preset by eye, or leave to the workflow?

**8 — Hand off.** One integration check, stated vs. inferred summary with receipts, revision is not confirmation. Then enter the workflow, install it if needed, and the workflow's Setup writes `BRIEF.md`.

## BRIEF.md frontmatter

| Key | Meaning | Example |
| --- | --- | --- |
| `workflow` | the executing workflow | `remotion-explainer` |
| `flow` | `automation` or `companion` | `automation` |
| `storyboard` | `yes` or `no` | `yes` |
| `message` | the ONE thing the video must communicate | `"Ship it in an afternoon"` |
| `destination` / `aspect` / `language` / `audience` / `length` | registry fields | — |
