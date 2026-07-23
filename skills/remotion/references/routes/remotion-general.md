# Route: remotion-general

- **Input:** Any video request that does not match a more specific route, OR a companion-mode request from any other route.
- **Output:** A custom Remotion composition built collaboratively or autonomously from the confirmed brief, using the full Remotion capability set.
- **Triggers:** Anything not matched by routes 1–9, OR `flow: companion` from any route.

## Contract

- Requires: `message` (the one thing the video must communicate), `destination`, `aspect`
- Optional: all other brief fields; the workflow negotiates them

## Interview

Run the full intent layer. No fields are pre-answered for this route. Use `capability-menu.md` to present the full capability set during the pitch round and nice-to-have step.

All four `flow × storyboard` combinations are valid. For long or complex briefs, recommend `storyboard: yes`.
