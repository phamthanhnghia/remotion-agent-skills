# Spring Adapter Reference

## spring() — the primary animation primitive

`spring()` computes a physics-based animation value. Given a frame index and FPS, it returns a value that naturally decelerates from 0 toward 1 (or from `from` toward `to`).

```tsx
import {spring, useCurrentFrame, useVideoConfig} from 'remotion';

const frame = useCurrentFrame();
const {fps} = useVideoConfig();

// progress: 0 → 1 (critically damped, no bounce)
const progress = spring({frame, fps, config: {damping: 200}});
```

## Config reference

| Parameter | Range | Effect |
| --- | --- | --- |
| `damping` | 1–500 | Resistance. Higher = less bounce. 200 = no overshoot. 8 = very bouncy. |
| `stiffness` | 1–1000 | Spring strength. Higher = faster. 200 = default. |
| `mass` | 0.1–100 | Inertia. Higher = slower, heavier feel. |

### Presets

```tsx
// Snappy entrance (default, no bounce)
spring({frame, fps, config: {damping: 200, stiffness: 200, mass: 1}})

// Bouncy entrance
spring({frame, fps, config: {damping: 8, stiffness: 300, mass: 1}})

// Gentle, slow entrance
spring({frame, fps, config: {damping: 30, stiffness: 80, mass: 2}})

// Ultra snappy (for quick pops)
spring({frame, fps, config: {damping: 500, stiffness: 400, mass: 0.5}})
```

## durationInFrames — cap the spring

`durationInFrames` caps the spring so it reaches 1.0 at exactly that frame:

```tsx
const progress = spring({
  frame,
  fps,
  config: {damping: 200},
  durationInFrames: 30, // guaranteed to finish in 30 frames
});
```

Use this when you need predictable, capped durations rather than physics-determined end times.

## delay — stagger pattern

Offset the spring start by delaying the frame input:

```tsx
// Spring starts at frame 15
const progress = spring({
  frame: Math.max(0, frame - 15),
  fps,
  config: {damping: 200},
});
```

Always use `Math.max(0, frame - delay)` — never pass a negative frame to `spring()`.

## from and to — custom value range

```tsx
// Scale from 0.8 to 1.0 (subtle pop, not full 0→1)
const scale = spring({frame, fps, from: 0.8, to: 1.0, config: {damping: 200}});

// Y from 40 to 0 (slide in from below)
const y = spring({frame, fps, from: 40, to: 0, config: {damping: 200}});
```

## Exit animation (reverse spring)

Springs go 0→1. To animate an exit (1→0), reverse-compute the exit frame:

```tsx
const {durationInFrames} = useVideoConfig();
const EXIT_DURATION = 20;

// Exit spring: progress goes 0→1 as the exit plays
const exitProgress = spring({
  frame: Math.max(0, (durationInFrames - EXIT_DURATION) - frame + EXIT_DURATION - (durationInFrames - frame)),
  fps,
  config: {damping: 200},
});

// Simpler: use interpolate for exit opacity
const exitOpacity = interpolate(
  frame,
  [durationInFrames - EXIT_DURATION, durationInFrames],
  [1, 0],
  {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}
);
```

For exits, `interpolate()` is usually simpler and preferred over reverse-spring math.

## measureSpring — compute duration from config

```tsx
import {measureSpring} from 'remotion';

// How many frames until the spring reaches 99.9% of its final value?
const frames = measureSpring({
  fps: 30,
  config: {damping: 200, stiffness: 200, mass: 1},
  threshold: 0.001,
}); // typically 20–25 frames
```

Use `measureSpring` to size `durationInFrames` on `<Composition>` or `<Sequence>` when entrance animation drives the minimum scene duration.
