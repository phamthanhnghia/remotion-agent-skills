# Determinism Rules

A Remotion composition must be a **pure function of frame → pixels**. The renderer may capture frames in parallel across multiple browser instances. Any code that reads external mutable state produces different pixels on different threads — a non-deterministic render.

## Banned at render time

| Banned | Why | Alternative |
| --- | --- | --- |
| `Math.random()` | Different value per thread | `random(seed)` from `remotion` |
| `Date.now()` | Different value per frame | `useCurrentFrame()` |
| `performance.now()` | Same as above | `useCurrentFrame()` |
| `setTimeout` / `setInterval` | Async, does not seek | `calculateMetadata` for one-time async |
| CSS transitions | Independent of frame | `spring()` / `interpolate()` |
| CSS animations | Same as above | `spring()` / `interpolate()` |
| `requestAnimationFrame` | Async | Not needed — Remotion drives frames |
| `useState` for visual values | Stateful, not seekable | Derive from frame directly |
| `useEffect` for visual values | Same as above | Derive from frame directly |
| `fetch()` at render time | Network unavailable in parallel render | `delayRender` / `calculateMetadata` |
| Infinite loops | Hangs renderer | Use finite `repeat` or animation controlled by frame |

## Allowed

- `spring({frame, fps, config})` — deterministic physics
- `interpolate(frame, inputRange, outputRange, options)` — deterministic mapping
- `random(seed)` — seeded pseudorandom, stable across threads
- `useCurrentFrame()` — the single source of time truth
- `useVideoConfig()` — stable composition config (fps, width, height, durationInFrames)
- `calculateMetadata` — async, runs once before render, safe for data fetching
- `delayRender` / `continueRender` — async asset loading before first frame

## Single concurrency debugging

If you suspect non-determinism (flickering, inconsistent frames):

```bash
npx remotion render src/index.ts MyComp out/test.mp4 --concurrency=1
```

If the issue disappears with `--concurrency=1`, the composition has non-deterministic code.
