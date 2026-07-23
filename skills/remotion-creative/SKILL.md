---
name: remotion-creative
description: >
  Non-animation creative direction for Remotion videos. Use for design spec handling, palettes,
  typography, narration, beat planning, audio-reactive visuals, composition patterns, and brand/style
  decisions. For spring/interpolate motion patterns and scene blueprints, use `remotion-animation`.
---

# Remotion Creative

Brand, pacing, style, narration, and composition direction. Use after the technical contract from `remotion-core` is in place.

For motion patterns, spring blueprints, transitions, and text animation effects, use `remotion-animation` — this skill is intentionally non-animation.

> **Read these two FIRST for any non-trivial composition:**
>
> - `references/house-style.md` — interpret the prompt, generate real content, the lazy-default list, and the background/foreground layer recipe
> - `references/video-composition.md` — video-medium scale, depth, and foreground detail; how to avoid empty layouts without imposing a universal element count
>
> Skipping these is the single biggest cause of generic output.

## Workflow

1. If a project has a design spec, **read it first** and treat its frontmatter tokens as brand truth (colors, fonts, spacing, tone). Resolution order: `frame.md` → `design.md` → `DESIGN.md`.
2. If no design spec exists and the user asks for visual direction:
   - Ready-made visual preset → `references/visual-styles.md`
   - Named style or mood → `references/visual-styles.md`
   - Fast defaults → `references/house-style.md`
3. For multi-scene work, plan beats and rhythm before writing code → `references/beat-direction.md`.
4. For motion-heavy work, read `references/motion-principles.md`, then go to `remotion-animation`.

## Routing

| Topic | Read |
| --- | --- |
| Default palettes, motion, typography, lazy defaults to question | `references/house-style.md` |
| Named style presets, mood-to-style routing | `references/visual-styles.md` |
| Composition patterns — PiP, text-behind-subject, title card, slide show | `references/composition-patterns.md` |
| Stats / infographic presentation | `references/data-in-motion.md` |
| Video-medium density, scale, color, frame composition | `references/video-composition.md` |
| Per-beat direction, rhythm planning, transition timing | `references/beat-direction.md` |
| High-level motion guardrails and spring quality rules | `references/motion-principles.md` |
| Font selection, pairings, rendered-video type guardrails | `references/typography.md` |
| Script pacing, tone, openings, number pronunciation | `references/narration.md` |
| Precomputed audio bands mapped to motion | `references/audio-reactive.md` |

## Design System in Remotion

Remotion compositions use React inline styles — there is no CSS cascade from the browser's user agent stylesheet in the rendered output. Always:

- Declare `fontFamily` explicitly on text elements (load via `@remotion/google-fonts` or `staticFile`)
- Use `rem`/`em` relative units cautiously; prefer `px` for pixel-perfect video output
- Import Google Fonts via `@remotion/google-fonts` — never via a `<link>` tag that requires a network call during render

```tsx
import {loadFont} from '@remotion/google-fonts/Inter';

const {fontFamily} = loadFont();

const Title = () => (
  <div style={{fontFamily, fontSize: 72, fontWeight: 700, color: '#ffffff'}}>
    Hello
  </div>
);
```

## Audio-reactive motion

Pre-extract audio bands before authoring — do not read audio files at render time.

Use `@remotion/media-utils` to pre-analyze audio:

```bash
# Pre-extract audio data to a JSON file
node scripts/extract-audio-data.mjs public/music.mp3 --out public/audiomap.json
```

Then import the JSON statically and map band values to animation parameters. Full workflow → `references/audio-reactive.md`.

## Boundaries

- Do not override `remotion-core` technical rules.
- Do not require a design system for a minimal technical composition.
- Do not add extra scenes, narration, music, captions, or transitions unless the request calls for them or you first propose the expansion.
- Keep recipe references task-specific; do not read every reference for simple edits.
