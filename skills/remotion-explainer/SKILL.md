---
name: remotion-explainer
description: >
  Explainer video workflow for Remotion: teach concepts, topics, mechanisms, or articles with invented visuals.
  Use for narrated compositions explaining how something works, 30–180 seconds.
---

# Remotion Explainer Workflow

Author narrated explainer videos explaining a topic, article, blog post, mechanism, or concept using Remotion.

## Contract & Prerequisites

- **Input:** A topic, article, blog post, mechanism, or concept to explain. No existing footage required. The agent invents visuals to illustrate the explanation.
- **Output:** A narrated Remotion composition with text, icons, animated illustrations, and structured scenes, 30–180 s.
- **Triggers:** "explain X", "make an explainer about", "teach how X works", "walkthrough of X concept".

## Interview & Setup

1. **Topic / Concept:** What is the core subject to explain?
2. **Audience:** Beginner, Intermediate, or Expert (default: Beginner).
3. **Target Length:** 30s, 60s, 90s, or 120s (default: 60s).
4. **Language:** Target language for narration/text.
5. **Aspect Ratio:** 16:9 (YouTube), 9:16 (Shorts/Reel), 1:1 (LinkedIn).

## Workflow Execution

1. Write `BRIEF.md` confirming the workflow intent.
2. Author `STORYBOARD.md` breaking the explainer into clear 10–20s scenes.
3. Build composition components using `/remotion-core` and `/remotion-animation`.
4. Render & verify output via `/remotion-cli`.
