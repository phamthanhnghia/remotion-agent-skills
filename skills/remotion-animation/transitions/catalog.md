# Transition Catalog

Available transitions from `@remotion/transitions`. Import from `@remotion/transitions/<name>`.

## Presentations

| Import | Effect | Key options |
| --- | --- | --- |
| `fade()` | Cross-fade opacity | none |
| `wipe({direction})` | Hard edge wipe | `direction: 'left' \| 'right' \| 'up' \| 'down'` |
| `slide({direction})` | Scene slides in/out | `direction: 'left' \| 'right' \| 'up' \| 'down'` |
| `flip({direction})` | 3D flip around axis | `direction: 'left' \| 'right' \| 'up' \| 'down'` |
| `clockWipe()` | Radial clock wipe | none |
| `cube({direction})` | 3D cube rotation | `direction: 'left' \| 'right'` |
| `pageCurl()` | Page curl effect | none |
| `none()` | Instant cut | none |

## Timings

```tsx
import {linearTiming, springTiming} from '@remotion/transitions';

// Fixed duration linear
linearTiming({durationInFrames: 20})

// Physics-based spring (duration determined by config)
springTiming({
  config: {damping: 200},   // default: no bounce
  durationRestThreshold: 0.001,
})
```

## Example: mixed transitions

```tsx
<TransitionSeries>
  <TransitionSeries.Sequence durationInFrames={90}>
    <Intro />
  </TransitionSeries.Sequence>

  {/* Fade for smooth intro→main transition */}
  <TransitionSeries.Transition
    presentation={fade()}
    timing={linearTiming({durationInFrames: 15})}
  />

  <TransitionSeries.Sequence durationInFrames={120}>
    <Main />
  </TransitionSeries.Sequence>

  {/* Wipe for energetic main→outro transition */}
  <TransitionSeries.Transition
    presentation={wipe({direction: 'left'})}
    timing={springTiming({config: {damping: 12, stiffness: 300}})}
  />

  <TransitionSeries.Sequence durationInFrames={60}>
    <Outro />
  </TransitionSeries.Sequence>
</TransitionSeries>
```
