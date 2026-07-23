# interpolate Adapter Reference

## interpolate() — timed value mapping

`interpolate()` maps an input value from one range to another. The most common use: map `frame` to a visual property value.

```tsx
import {interpolate} from 'remotion';

const opacity = interpolate(
  frame,        // input
  [0, 30],      // input range (frame 0 → frame 30)
  [0, 1],       // output range (opacity 0 → 1)
  {
    extrapolateLeft: 'clamp',   // behavior outside the input range
    extrapolateRight: 'clamp',
  }
);
```

## Extrapolation modes

| Mode | Behavior |
| --- | --- |
| `'clamp'` | Value stays at boundary when input is outside range. **Use this for almost everything.** |
| `'extend'` | Continues the linear trend beyond the range. Useful for continuous panning. |
| `'identity'` | Returns the input value itself when outside range. Rarely used. |
| `'wrap'` | Wraps around. Useful for repeating cycles. |

Default is `'extend'` — **always specify `extrapolateRight: 'clamp'` for entrances** to prevent values from continuing past the end of the animation.

## Easing

Pass an easing function as `easing` option. Remotion exports standard easings via `Easing`:

```tsx
import {interpolate, Easing} from 'remotion';

const x = interpolate(frame, [0, 60], [0, 300], {
  extrapolateRight: 'clamp',
  easing: Easing.bezier(0.25, 0.1, 0.25, 1), // ease-in-out cubic bezier
});
```

**Available easings:**

```tsx
Easing.linear         // no easing
Easing.ease           // CSS ease equivalent
Easing.in(fn)         // easing in
Easing.out(fn)        // easing out (recommended for exits)
Easing.inOut(fn)      // ease in and out
Easing.bezier(x1,y1,x2,y2)  // custom cubic bezier
Easing.elastic(bounciness)   // elastic bounce (use sparingly)
Easing.back(overshoot)       // slight overshoot
Easing.bounce                // bouncy end
```

## Multi-stop sequences

```tsx
// Pop scale: 0 → 1.2 → 0.95 → 1.0
const scale = interpolate(
  frame,
  [0, 8, 14, 20],          // input stops
  [0, 1.2, 0.95, 1.0],     // output stops
  {extrapolateRight: 'clamp'}
);
```

Each consecutive pair of stops defines a linear segment. Use `easing` on the whole call or use the `Easing` import for bezier curves.

## Common recipes

```tsx
// Fade in (frames 0–20)
const opacity = interpolate(frame, [0, 20], [0, 1], {extrapolateRight: 'clamp'});

// Fade out (last 20 frames of durationInFrames)
const {durationInFrames} = useVideoConfig();
const fadeOut = interpolate(frame, [durationInFrames - 20, durationInFrames], [1, 0], {extrapolateLeft: 'clamp'});

// Slide in from left (frames 10–40)
const x = interpolate(frame, [10, 40], [-300, 0], {
  extrapolateLeft: 'clamp',
  extrapolateRight: 'clamp',
  easing: Easing.out(Easing.quad),
});

// Scale from 0 to 1 (frames 0–30)
const scale = interpolate(frame, [0, 30], [0, 1], {
  extrapolateRight: 'clamp',
  easing: Easing.out(Easing.back(1.2)),
});

// Color lerp (r, g, b independently)
const r = Math.round(interpolate(frame, [0, 60], [30, 99], {extrapolateRight: 'clamp'}));
const g = Math.round(interpolate(frame, [0, 60], [30, 102], {extrapolateRight: 'clamp'}));
const b = Math.round(interpolate(frame, [0, 60], [212, 241], {extrapolateRight: 'clamp'}));
const color = `rgb(${r},${g},${b})`;
```
