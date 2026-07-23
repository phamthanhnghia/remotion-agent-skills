---
name: remotion-animation
description: >
  All animation knowledge for Remotion — atomic motion rules using interpolate/spring, multi-phase
  scene blueprints, scene transitions, broader motion-design techniques. Covers spring physics tuning,
  interpolate extrapolation modes, stagger patterns, entrance/exit choreography, text animations,
  and layout-driven motion. HyperFrames-style: frame-deterministic, seekable, no CSS transitions.
---

# Remotion Animation

All motion knowledge in one skill: **rules** (atomic recipes), **blueprints** (multi-phase scene templates), **transitions** (scene-to-scene), and **techniques** (broader motion-design patterns).

For the composition contract (useCurrentFrame, Sequence, determinism) see `remotion-core`.

## Default: compose atomic rules

Pick 2–4 rules from `rules-index.md`, express them with `interpolate()` and `spring()` from a single `useCurrentFrame()` call, done. This is faster and produces less code than starting from a blueprint.

## Load a blueprint when

- The scene matches an existing pre-designed multi-phase template (brand-reveal, data-story, social-proof, etc.) and reusing its phase pipeline saves real authoring time
- You want runnable ground-truth code for a complex 4–5 phase choreography

Blueprints live in `blueprints-index.md`. Each entry points to `blueprints/<id>.md` (recipe). Load it only when you've decided you need scene-level orchestration.

## Routing

| Want to… | Read |
| --- | --- |
| Pick an atomic motion pattern by trigger / tag | `rules-index.md` |
| Read one rule's full TSX recipe | `rules/<name>.md` |
| Pick a multi-phase scene template | `blueprints-index.md` |
| Read one blueprint's full recipe | `blueprints/<id>.md` |
| Author a scene transition (interpolate-driven, between two Sequences) | `transitions/overview.md`, `transitions/catalog.md` |
| Look up a broader motion-design technique | `techniques.md` |
| `spring()` — physics config, damping, stiffness, mass | `adapters/spring.md` |
| `interpolate()` — extrapolation, input/output ranges, clamp | `adapters/interpolate.md` |
| Stagger — indexed delay patterns, spring stagger | `adapters/stagger.md` |
| Text animations — word-by-word, char-by-char, line reveals | `adapters/text-animation.md` |
| SVG path animations, stroke draw, shape morph via CSS | `adapters/svg.md` |
| Three.js / R3F — 3D scenes in React, AnimationMixer seek | `adapters/three.md` |
| Lottie — After Effects exports via @remotion/lottie | `adapters/lottie.md` |

## Picking an animation primitive

- **`spring()`** is the default for entrance motions — provides natural deceleration, no easing curve math
- **`interpolate()`** is the default for timed value mapping — opacity fade, position lerp, color transitions
- **Stagger** — map an array index to a delayed `spring()` or delayed `interpolate()` frame offset
- **CSS keyframes** — only for infinite decorative loops (spinners, shimmer) where seek doesn't matter
- **Three.js / R3F** — for 3D scenes; use `useCurrentFrame` to drive `AnimationMixer.setTime(frame / fps)`
- **Lottie via @remotion/lottie** — for After Effects exports; `LottieAnimationData` + `Lottie` component

Multiple primitives can coexist in one composition. Each derives from the same `useCurrentFrame()`.

## Critical Constraints

**Prerequisite: `remotion-core` → Non-Negotiable Rules** (frame-deterministic, no `Math.random`, no `Date.now`, no CSS transitions for timeline-synchronized motion, no `useState`/`useEffect` for visual properties, all values derived from `useCurrentFrame()`).

Animation-craft additions on top of core's contract:

- **Pre-calculated layout constants** — never call `getBoundingClientRect()` at render time. DOM measurements are unreliable during parallel frame rendering; compute coordinates from composition `width`/`height` via `useVideoConfig()` instead
- **Spatial motion uses React inline styles** — `transform: translateX(${x}px)` not `left`/`top` (avoid layout recalc)
- **`spring()` never re-evaluates at different times** — the same `frame` always returns the same value for the same config
- **`interpolate()` with `clamp` extrapolation** for entrance/exit to prevent values leaking beyond scene boundaries

## Core Animation Patterns

### Entrance with spring

```tsx
const frame = useCurrentFrame();
const {fps} = useVideoConfig();

const scale = spring({frame, fps, config: {damping: 12, stiffness: 200}});
const opacity = interpolate(frame, [0, 15], [0, 1], {extrapolateRight: 'clamp'});
```

### Timed interpolation

```tsx
const x = interpolate(frame, [30, 60], [-200, 0], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

### Stagger pattern

```tsx
const items = ['A', 'B', 'C', 'D'];
// Each item enters 8 frames after the previous
const getItemSpring = (index: number) =>
  spring({frame: frame - index * 8, fps, config: {damping: 200}});
```

### Scene exit

```tsx
const {durationInFrames} = useVideoConfig();
const exitStart = durationInFrames - 20;
const opacity = interpolate(frame, [exitStart, durationInFrames], [1, 0], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

## See Also

- `remotion-core` — composition structure, determinism contract, Sequence, media elements
- `remotion-creative` — palettes, typography, narration, beat planning
- `remotion-cli` — `npx remotion render / studio / lambda`
- `remotion-keyframes` — detailed spring configs, interpolate recipes, advanced stagger
