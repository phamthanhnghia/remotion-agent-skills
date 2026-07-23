# Route: remotion-slideshow

- **Input:** A set of slides, a presentation outline, a pitch deck brief, or a list of topics/sections.
- **Output:** A slide-by-slide Remotion composition with animated slide transitions, title slides, content slides, and a closing slide.
- **Triggers:** "make a presentation", "pitch deck video", "slide show", "turn my outline into a video presentation".

## Contract

- Requires: `message` (the deck's core thesis), `slides` (count or outline), `destination`, `aspect`
- Optional: `length`, `transition_style`, `style_preset`, narration

## Interview

1. **What is the deck about and what should it convince the audience of?** (must-have)
2. **How many slides or sections?** (or paste the outline)
3. **Should each slide have narration?** (yes / no — defer TTS selection)
4. **Target platform?** (16:9 / 9:16 — recommend 16:9 for presentations)
5. Run-shape questions: (a) storyboard? (b) automation or companion?
