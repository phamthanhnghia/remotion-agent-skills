# Timing API Reference

## useCurrentFrame

Returns the current frame index (0-indexed). This is the **only** legitimate source of time in a Remotion composition.

```tsx
import {useCurrentFrame} from 'remotion';

const frame = useCurrentFrame(); // 0 at the start, durationInFrames-1 at the end
```

Inside a `<Sequence from={30}>`, `useCurrentFrame()` returns 0 when the parent composition is at frame 30. Sequences give children a local frame-0.

## useVideoConfig

Returns the composition config: `fps`, `width`, `height`, `durationInFrames`.

```tsx
import {useVideoConfig} from 'remotion';

const {fps, width, height, durationInFrames} = useVideoConfig();
const durationInSeconds = durationInFrames / fps;
```

## interpolate

Maps an input value to an output range. Commonly used to map frame index to a visual property.

```tsx
import {interpolate} from 'remotion';

const opacity = interpolate(
  frame,            // input value
  [0, 30],          // input range
  [0, 1],           // output range
  {
    extrapolateLeft: 'clamp',    // clamp | extend | identity | wrap
    extrapolateRight: 'clamp',
    easing: (t) => t * t,       // optional easing function
  }
);
```

**Multi-stop interpolation:**

```tsx
const y = interpolate(
  frame,
  [0, 20, 40, 60],      // multiple input stops
  [0, -30, 10, 0],      // corresponding output stops
  {extrapolateRight: 'clamp'}
);
```

## spring

Physics-based animation primitive. Returns a value that starts at 0 and approaches 1 as frames increase.

```tsx
import {spring, useCurrentFrame, useVideoConfig} from 'remotion';

const frame = useCurrentFrame();
const {fps} = useVideoConfig();

const progress = spring({
  frame,
  fps,
  config: {
    damping: 200,    // higher = less bounce (200 = critically damped)
    stiffness: 200,  // higher = faster spring
    mass: 1,         // higher = slower, heavier feel
  },
  durationInFrames: 30,  // optional: cap spring at N frames
  delay: 10,             // optional: offset spring start by N frames
});

// Use progress (0→1) to drive transform:
const scale = progress; // or: 0.5 + 0.5 * progress (for 0.5→1 scale)
```

**Delayed spring (stagger pattern):**

```tsx
const delayedProgress = spring({
  frame: Math.max(0, frame - delayInFrames),
  fps,
  config: {damping: 200},
});
```

## measureSpring

Computes how many frames a spring with a given config needs to reach its final value. Use to calculate scene durations.

```tsx
import {measureSpring} from 'remotion';

const frames = measureSpring({
  fps: 30,
  config: {damping: 200, stiffness: 200},
  threshold: 0.001, // within 0.1% of final value
});
// → typically 20–30 frames for damping:200
```
