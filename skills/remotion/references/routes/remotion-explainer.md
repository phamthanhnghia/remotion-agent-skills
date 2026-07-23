# Route: remotion-explainer

- **Input:** A topic, article, blog post, mechanism, or concept to explain. No existing footage required. The agent invents visuals to illustrate the explanation.
- **Output:** A narrated Remotion composition with text, icons, and animated illustrations explaining the topic, 30–180 s.
- **Triggers:** "explain X", "make an explainer about", "teach how X works", "walkthrough of X concept".

## Contract

- Requires: `message` (the one thing to communicate), `audience`, `language`, `aspect`, `destination`
- Optional: `length`, `voice`, `style_preset`, narration script

## Interview

Ask the following, one question per field, recommended option first:

1. **What is the concept or topic?** (must-have — if already given, skip)
2. **Who is the audience?** (beginner / intermediate / expert — recommend beginner)
3. **Target length?** (30 s / 60 s / 90 s / 120 s — recommend 60 s)
4. **Narration language?** (recommend user's language from context)
5. **Destination platform?** (YouTube 16:9 / Instagram Reel 9:16 / LinkedIn 1:1 — recommend 16:9)
6. Run-shape questions: (a) storyboard? (b) automation or companion?

Do not ask about voice or style at the must-have stage — defer to the deferred-asks list after concept confirmation.
