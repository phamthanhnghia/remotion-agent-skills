---
name: hyperframes-to-remotion
description: >
  Migration workflow from HyperFrames to Remotion: convert HyperFrames HTML compositions to React Remotion code.
  Use when translating HTML GSAP timelines into Remotion frame-deterministic React components.
---

# HyperFrames to Remotion Migration Workflow

Migrate legacy or existing HyperFrames HTML compositions to pure React Remotion code.

## Mapping Table

| HyperFrames Concept | Remotion Equivalent |
| --- | --- |
| GSAP Timeline `.to()` / `.from()` | `useCurrentFrame()` + `spring()` / `interpolate()` |
| `<div class="clip" data-start data-end>` | `<Sequence from={start} durationInFrames={duration}>` |
| `<video>` element | `<OffthreadVideo src={staticFile('...')} />` |
| `<audio>` element | `<Audio src={staticFile('...')} />` |
| `npx hyperframes render` | `npx remotion render src/index.ts MyComp out/video.mp4` |

## Migration Steps

1. Parse HyperFrames HTML structure and timeline values.
2. Re-create scene layouts as React `<AbsoluteFill>` components.
3. Replace GSAP interpolations with frame-deterministic Remotion `spring()` and `interpolate()` primitives.
4. Verify output in Remotion Studio.
