# Transitions Overview

Scene transitions in Remotion are implemented by **overlapping two `<Sequence>` components** and applying frame-driven opacity, transform, or clip-path to create a visual handoff. Since there is no global timeline object (unlike GSAP-based HyperFrames), transitions live as pure frame computations in the parent composition.

## The overlap pattern

```tsx
const TRANSITION_DURATION = 20; // frames

// Scene A ends at frame 90
// Scene B starts at frame 70 (overlap of 20 frames)
// Transition zone: frames 70–90

<Sequence from={0} durationInFrames={90}>
  <SceneA />
</Sequence>
<Sequence from={70} durationInFrames={90}>
  <SceneB />
</Sequence>
```

To compute the transition progress inside each scene:

```tsx
// In SceneA (last TRANSITION_DURATION frames of its Sequence):
const {durationInFrames} = useVideoConfig();
const exitProgress = interpolate(
  frame,
  [durationInFrames - TRANSITION_DURATION, durationInFrames],
  [0, 1],
  {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}
);

// In SceneB (first TRANSITION_DURATION frames of its Sequence):
const enterProgress = interpolate(
  frame,
  [0, TRANSITION_DURATION],
  [0, 1],
  {extrapolateRight: 'clamp'}
);
```

## @remotion/transitions (recommended for most cases)

```bash
npm install @remotion/transitions
```

`@remotion/transitions` provides a `<TransitionSeries>` component that handles the overlap math automatically:

```tsx
import {TransitionSeries, linearTiming, springTiming} from '@remotion/transitions';
import {fade} from '@remotion/transitions/fade';
import {wipe} from '@remotion/transitions/wipe';
import {slide} from '@remotion/transitions/slide';

<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={90}>
    <SceneA />
  </TransitionSeries.Sequence>

  <TransitionSeries.Transition
    presentation={fade()}           // which transition effect
    timing={linearTiming({durationInFrames: 20})}
  />

  <TransitionSeries.Sequence durationInFrames={90}>
    <SceneB />
  </TransitionSeries.Sequence>

  <TransitionSeries.Transition
    presentation={wipe({direction: 'left'})}
    timing={springTiming({config: {damping: 200}})}
  />

  <TransitionSeries.Sequence durationInFrames={90}>
    <SceneC />
  </TransitionSeries.Sequence>
</TransitionSeries>
```

Read `transitions/catalog.md` for all available presentations and their options.

## When to use @remotion/transitions vs. manual overlap

| Use | Choice |
| --- | --- |
| Standard cut, fade, wipe, slide | `@remotion/transitions` |
| Custom shader or complex visual effect | Manual overlap with custom frame math |
| Precise timing control with spring physics | `@remotion/transitions` with `springTiming` |
| Matching transition to beat grid | Manual overlap, compute overlap from beat data |
