# Route: hyperframes-to-remotion

- **Input:** Existing HyperFrames HTML composition source, only when the user explicitly asks to port, convert, or migrate it.
- **Output:** A Remotion React composition translated from the source.
- **Triggers:** "port my HyperFrames project to Remotion", "convert this HyperFrames composition", "migrate from HyperFrames to Remotion".

## Contract

- Input: `index.html` (HyperFrames standalone composition) or sub-composition HTML
- Output: React component in `src/`, registered in `src/Root.tsx`, with equivalent timing and motion

## Migration mapping

| HyperFrames | Remotion equivalent |
| --- | --- |
| `data-duration` (seconds) | `durationInFrames = duration * fps` on `<Composition>` |
| `data-start`, `data-end` | `<Sequence from={start * fps} durationInFrames={(end - start) * fps}>` |
| `class="clip"` | `<Sequence>` child — visibility controlled by timeline position |
| `data-track-index` | z-order via React render order and `position: absolute` |
| `window.__timelines[id]` GSAP | `spring()` / `interpolate()` driven by `useCurrentFrame()` |
| `gsap.timeline({ paused: true })` | no equivalent — all values computed inline from frame |
| `data-composition-src` | separate React component imported and rendered in `<Sequence>` |
| `<video>` / `<audio>` | `<OffthreadVideo>` / `<Audio>` with `startFrom` / `endAt` |

## Interview

Not served by the intent layer — a migration with no brief. Route directly to this workflow.
