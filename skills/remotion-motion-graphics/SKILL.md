---
name: remotion-motion-graphics
description: >
  Short motion graphics workflow for Remotion: unnarrated visual hit under 10 seconds.
  Use for logo stings, animated titles, lower thirds, transition stingers, and social bumpers.
---

# Remotion Motion Graphics Workflow

Author ultra-short, highly polished unnarrated motion graphic units under 10 seconds.

## Contract & Prerequisites

- **Input:** Title text, logo, stat hit, or short announcement text. Unnarrated.
- **Output:** Frame-deterministic motion graphic loop or stinger, 3–10 s (90–300 frames @ 30fps).
- **Triggers:** "logo sting", "animated title card", "lower third", "motion graphic bumper".

## Technical Guidelines

- Derive all movement from `spring()` and `interpolate()`.
- Use `@remotion/google-fonts` for typography.
- Implement smooth entrance & exit spring physics.
