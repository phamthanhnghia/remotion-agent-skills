---
name: remotion-keyframes
description: Keyframe animation engine for Remotion. Use for precise frame-level animation, interpolate curves, stagger patterns, entrance/exit choreography, and text reveal animations.
---

# Remotion Keyframes

Keyframes are a pose contract: visible states, continuous subject identity, frame-deterministic, verified pixels.

Use `remotion-animation` for broad scene recipes. Use `remotion-cli` for full command docs.

## Procedure

1. Identify the animated subject, visible states, final state, and animation primitive (`spring` or `interpolate`).
2. Choose the smallest mechanism that proves the prompt.
3. Author frame-deterministic keyframes. All values derive from `useCurrentFrame()`.
4. Verify with `npx remotion studio` — scrub the full timeline, check first frame, all poses, and final frame.
5. If proof fails, fix the source keyframes and re-verify before rendering.

## Contract

- Name the moving subject.
- Name the poses needed to prove the intended motion, including the final state.
- Keyframe visible channels, not hidden helper state.
- Preserve object identity when continuity matters.
- Hold readable or semantic states long enough to see (minimum 10 frames for text, 6 frames for icon).
- Final frame is part of the animation, not cleanup.
- Do not reset to rest unless requested.
- Do not end on black unless requested.

## Runtime Rules

`interpolate()` and `spring()`:

- all values computed synchronously from `frame` — no async inside interpolate/spring calls
- `extrapolateLeft: 'clamp'` and `extrapolateRight: 'clamp'` to prevent value leaking outside intended range
- `spring({frame, fps, config})` — frame must be the Remotion `useCurrentFrame()` value, not a derived `Date.now()` derivative
- For entrances, offset spring: `spring({frame: frame - delay, fps, config})`; use `Math.max(0, frame - delay)` to avoid negative frames driving spring

Never use for render-critical motion:

- `Date.now()` or `performance.now()`
- unseeded `Math.random()` (use Remotion's `random(seed)` instead)
- CSS transitions or CSS animations (they don't seek)
- `requestAnimationFrame`
- `setTimeout` / `setInterval` in render path
- `useState` / `useEffect` for visual-property values

## Spring Config Reference

```tsx
import {spring, useCurrentFrame, useVideoConfig} from 'remotion';

const frame = useCurrentFrame();
const {fps} = useVideoConfig();

// Snappy entrance (default feel)
const scale = spring({frame, fps, config: {damping: 200}});

// Bouncy entrance
const bounce = spring({frame, fps, config: {damping: 8, stiffness: 300}});

// Very smooth, slow
const smooth = spring({frame, fps, config: {damping: 30, stiffness: 80, mass: 2}});

// Sharp exit (reverse spring — animate from 1 to 0)
const {durationInFrames} = useVideoConfig();
const exitFrame = durationInFrames - frame;
const exitScale = spring({frame: exitFrame, fps, config: {damping: 200}});
const scaleValue = 1 - (1 - exitScale) ; // goes from 1 down to 0
```

## Interpolate Recipes

```tsx
import {interpolate} from 'remotion';

// Fade in over 20 frames
const opacity = interpolate(frame, [0, 20], [0, 1], {extrapolateRight: 'clamp'});

// Slide in from left
const x = interpolate(frame, [0, 30], [-300, 0], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});

// Multi-stop color shift
const r = interpolate(frame, [0, 30, 60], [255, 100, 0], {extrapolateRight: 'clamp'});
const g = interpolate(frame, [0, 30, 60], [0, 150, 255], {extrapolateRight: 'clamp'});

// Scale pop
const scale = interpolate(frame, [0, 8, 12, 18], [0, 1.15, 0.95, 1], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
});
```

## Channels

Prefer compositor/visual channels: `transform: translateX/Y/Z`, `transform: scale/rotate/skew`, `opacity`, `borderRadius`, CSS vars, `clipPath`, SVG `strokeDashoffset`.

Avoid layout channels: `top/left/right/bottom`, `width/height`, `margin/padding` — they trigger reflow, are slow, and produce incorrect results when the renderer samples frames in parallel.

## Stagger Pattern

```tsx
const ITEM_COUNT = 5;
const STAGGER_FRAMES = 8; // delay between each item

const items = Array.from({length: ITEM_COUNT}, (_, i) => {
  const delayedFrame = Math.max(0, frame - i * STAGGER_FRAMES);
  const itemSpring = spring({frame: delayedFrame, fps, config: {damping: 200}});
  return {opacity: itemSpring, y: (1 - itemSpring) * 40};
});
```

## Text Animation Pattern

```tsx
const words = 'Hello Remotion World'.split(' ');

const WordReveal = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();

  return (
    <div style={{display: 'flex', gap: 12}}>
      {words.map((word, i) => {
        const delayedFrame = Math.max(0, frame - i * 6);
        const progress = spring({frame: delayedFrame, fps, config: {damping: 200}});
        return (
          <span
            key={i}
            style={{
              opacity: progress,
              transform: `translateY(${(1 - progress) * 20}px)`,
              display: 'inline-block',
            }}
          >
            {word}
          </span>
        );
      })}
    </div>
  );
};
```

## SVG Stroke Draw

```tsx
// SVG stroke-dasharray draw-on effect
const pathLength = 500; // measure with path.getTotalLength() in development
const drawProgress = interpolate(frame, [0, 60], [0, 1], {extrapolateRight: 'clamp'});

<path
  d="M 0 0 L 300 200"
  stroke="white"
  strokeWidth={4}
  fill="none"
  strokeDasharray={pathLength}
  strokeDashoffset={pathLength * (1 - drawProgress)}
/>
```

## 3D Depth (CSS Perspective)

```tsx
// CSS perspective 3D scene
<div style={{perspective: 800, perspectiveOrigin: '50% 50%'}}>
  <div
    style={{
      transformStyle: 'preserve-3d',
      transform: `rotateY(${interpolate(frame, [0, 60], [45, 0], {extrapolateRight: 'clamp'})}deg)`,
    }}
  >
    {/* 3D content */}
  </div>
</div>
```

## Mechanism Choice

| Need | Mechanism |
| --- | --- |
| Entrance with natural deceleration | `spring()` |
| Timed value transition | `interpolate()` |
| Multi-stop sequence | `interpolate()` with multiple input/output stops |
| Many items with ordered entrance | stagger: indexed `frame - i * delay` in `spring()` |
| Text reveal (word by word) | stagger spring per word index |
| Stroke grows or traces | `strokeDashoffset` via `interpolate()` |
| Shape reveal boundary | `clipPath` via `interpolate()` |
| 3D depth scene | CSS `perspective` + `rotateX/Y/Z` via `interpolate()` |

## Error Handling

| Failure | Fix |
| --- | --- |
| Values jump at certain frames | Check for negative `frame` passed to `spring()` — clamp with `Math.max(0, frame - delay)` |
| Animation doesn't seek correctly | Confirm no `useState`/CSS animation driving the value |
| Black frames at start or end | Check `extrapolateLeft: 'clamp'` on entrance, `extrapolateRight: 'clamp'` on exit |
| Text blinking / inconsistent | Ensure `random()` seed is a stable string, not `Math.random()` |
| Spring overshoots off-screen | Increase `damping` (200 = no overshoot, 8 = lots of bounce) |

## Done

Verify in Studio: scrub first frame, all visible poses, final frame. Confirm subject-owned motion, no debug overlays, no flash on first frame.
