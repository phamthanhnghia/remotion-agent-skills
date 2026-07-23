---
name: remotion-core
description: >
  The Remotion composition contract — build one renderable project. Use for composition structure,
  useCurrentFrame, useVideoConfig, interpolate, spring, Sequence, Series, OffthreadVideo, Audio,
  Img, AbsoluteFill, staticFile, deterministic-render rules, and validation. Also covers
  calculateMetadata, InputProps schema, and the STORYBOARD.md / SCRIPT.md plan formats.
  Read before writing any Remotion composition code.
---

# Remotion Core

Remotion renders video from React. A composition is a React component whose output is **a pure function of frame index** — the same frame always produces the same pixels. Every animation must be derived from `useCurrentFrame()`, never from `Date.now()`, `Math.random()`, or any external mutable state.

This skill is the **technical contract** — how to build one Remotion project. The body below is the build guide; per-topic detail lives in `references/` (index next), read on demand.

## References

| File | Read it to… |
| --- | --- |
| `references/minimal-composition.md` | start from the smallest renderable composition skeleton |
| `references/composition-patterns.md` | choose standalone vs nested; structure Root.tsx; use Sequence and Series |
| `references/timing-api.md` | use `useCurrentFrame`, `useVideoConfig`, `interpolate`, `spring`, `measureSpring` |
| `references/sequence-and-series.md` | time clips with `<Sequence from>`, chain scenes with `<Series>`, nested offsets |
| `references/media-elements.md` | place `<OffthreadVideo>`, `<Audio>`, `<Img>`, trim, volume, startFrom |
| `references/determinism-rules.md` | build a frame-deterministic composition; banned APIs; allowed animation primitives |
| `references/calculate-metadata.md` | declare duration/fps/size dynamically; fetch data before render; InputProps schema |
| `references/static-files.md` | reference assets via `staticFile()`; `getStaticFiles()`; public/ directory |
| `references/storyboard-format.md` | author a `STORYBOARD.md` plan |
| `references/review-loop.md` | run the plan → sketch → build review passes |
| `references/production-loop.md` | take an approved plan to a delivered video |
| `references/brief-format.md` | author `BRIEF.md` — the confirmed intent document |
| `references/script-format.md` | author the optional `SCRIPT.md` locked narration |

For animation specifics (spring physics, interpolate extrapolation, stagger) go to `remotion-animation` → `adapters/`.

## Building a composition

### The one root rule

Every composition entry file (`src/Root.tsx`) exports one `RemotionRoot` that registers compositions via `<Composition>`. Each `<Composition>` points to one React component — no global mutable state, no side effects at module load time.

```tsx
// src/Root.tsx
import {Composition} from 'remotion';
import {MyScene} from './MyScene';

export const RemotionRoot: React.FC = () => (
  <>
    <Composition
      id="MyScene"
      component={MyScene}
      durationInFrames={150}
      fps={30}
      width={1920}
      height={1080}
    />
  </>
);
```

### Frame-driven rendering (non-negotiable)

Every visual value is computed from `useCurrentFrame()`. The renderer captures each frame independently using potentially parallel browser instances — code that reads external mutable state produces different pixels on different threads.

```tsx
import {useCurrentFrame, useVideoConfig, interpolate, spring} from 'remotion';

const MyScene: React.FC = () => {
  const frame = useCurrentFrame();
  const {fps, durationInFrames} = useVideoConfig();

  const opacity = interpolate(frame, [0, 30], [0, 1], {extrapolateRight: 'clamp'});
  const scale = spring({frame, fps, config: {damping: 200}});

  return (
    <AbsoluteFill style={{opacity, transform: `scale(${scale})`}}>
      <h1>Hello Remotion</h1>
    </AbsoluteFill>
  );
};
```

### Non-negotiable rules (silent bugs automated gates may miss)

- **No `Math.random()`** — use `random(seed)` from `'remotion'` for seeded pseudorandomness
- **No `Date.now()` / `performance.now()`** — use `useCurrentFrame()` for all time-based values
- **No `setTimeout` / `setInterval`** in render path — only in `calculateMetadata`'s async scope
- **No `useState`/`useEffect` for visual properties** — derive all visual state from frame index
- **No CSS transitions/animations** — they run independently of Remotion's timeline seek
- **No infinite loops** — `repeat(-1)` or equivalent will hang the renderer
- **No `useRef` holding frame-derived mutable state** — refs reset on seek

Full rationale → `references/determinism-rules.md`.

### Composition sizing

Always provide explicit `width`, `height`, `fps`, and `durationInFrames` on `<Composition>`. These can be dynamic via `calculateMetadata` when driven by props.

### AbsoluteFill — the scene root

`<AbsoluteFill>` is `position: absolute; top: 0; left: 0; width: 100%; height: 100%` — use it as the scene root instead of a manual div. It guarantees full-bleed content and correct compositing.

```tsx
import {AbsoluteFill} from 'remotion';

const MyScene = () => (
  <AbsoluteFill style={{backgroundColor: '#0f0f0f'}}>
    {/* scene content */}
  </AbsoluteFill>
);
```

### Sequence — the clip primitive

`<Sequence from={startFrame} durationInFrames={duration}>` offsets the timeline for its children. Children see frame 0 at the parent's `from` frame. This is how scenes are composed without hardcoding frame math everywhere.

```tsx
import {Sequence} from 'remotion';

const Timeline = () => (
  <>
    <Sequence from={0} durationInFrames={60}>
      <IntroScene />
    </Sequence>
    <Sequence from={60} durationInFrames={90}>
      <MainScene />
    </Sequence>
    <Sequence from={150} durationInFrames={60}>
      <OutroScene />
    </Sequence>
  </>
);
```

### Media elements

- `<OffthreadVideo>` — use for video files; renders each frame off the main thread; supports `startFrom`, `endAt`, `volume`
- `<Audio>` — audio files; supports `startFrom`, `endAt`, `volume`
- `<Img>` — images; use `staticFile()` for public/ assets

All media paths must go through `staticFile('filename')` for assets in the `public/` directory.

## Editing existing compositions

- Read the files first. Preserve unrelated timing, IDs, props interfaces.
- Match existing composition IDs and component signatures.
- Adding a scene: use a non-overlapping `<Sequence from>` offset.
- `calculateMetadata` runs once before render — never call it at render time.

## Validation

Use `remotion-cli` for command details:

- [ ] `npx remotion studio` opens with no console errors
- [ ] All compositions visible and seekable in the timeline
- [ ] `npx remotion render <entry> <id> out/test.mp4 --concurrency=1` produces a non-empty file
- [ ] `ffprobe out/test.mp4` reports expected duration and resolution
- [ ] Render only after the user approves the preview
