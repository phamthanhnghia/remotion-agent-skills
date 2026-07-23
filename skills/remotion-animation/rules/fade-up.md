# Rule: fade-up

Element fades in while rising from below. Use for headlines, labels, and content blocks entering a scene.

## Recipe

```tsx
import {interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

type FadeUpProps = {
  delay?: number;       // frame offset before animation starts
  duration?: number;    // frames for the fade-up motion (default: 25)
  distance?: number;    // px to travel upward (default: 40)
};

const withFadeUp = (frame: number, fps: number, opts: FadeUpProps = {}) => {
  const {delay = 0, duration = 25, distance = 40} = opts;
  const adjustedFrame = Math.max(0, frame - delay);

  const opacity = interpolate(adjustedFrame, [0, duration * 0.6], [0, 1], {
    extrapolateRight: 'clamp',
  });

  const y = interpolate(adjustedFrame, [0, duration], [distance, 0], {
    extrapolateRight: 'clamp',
    easing: (t) => 1 - Math.pow(1 - t, 2), // ease-out quad
  });

  return {opacity, transform: `translateY(${y}px)`};
};

// Usage
const MyHeadline: React.FC = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();

  const fadeUpStyle = withFadeUp(frame, fps, {delay: 10, distance: 60});

  return (
    <h1 style={{color: 'white', fontSize: 96, ...fadeUpStyle}}>
      Hello World
    </h1>
  );
};
```

## Stagger variant

```tsx
// Stagger multiple elements
{items.map((item, i) => {
  const style = withFadeUp(frame, fps, {delay: i * 8, distance: 30});
  return <div key={i} style={{...style}}>{item}</div>;
})}
```
