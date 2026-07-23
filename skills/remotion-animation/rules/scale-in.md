# Rule: scale-in

Element scales from 0 to 1 with a spring-based natural deceleration. Use for icons, badges, thumbnails, and isolated elements that need to "pop" into existence.

## Recipe

```tsx
import {spring, useCurrentFrame, useVideoConfig} from 'remotion';

type ScaleInProps = {
  delay?: number;
  damping?: number;   // 200 = no bounce, 8 = bouncy
  stiffness?: number;
};

const withScaleIn = (frame: number, fps: number, opts: ScaleInProps = {}) => {
  const {delay = 0, damping = 200, stiffness = 200} = opts;
  const adjustedFrame = Math.max(0, frame - delay);

  const scale = spring({
    frame: adjustedFrame,
    fps,
    config: {damping, stiffness},
  });

  return {
    opacity: Math.min(1, scale * 2), // fade in quickly, finish before scale does
    transform: `scale(${scale})`,
    transformOrigin: 'center center',
  };
};

// Usage
const MyIcon: React.FC = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();

  const scaleStyle = withScaleIn(frame, fps, {delay: 0, damping: 12}); // slight bounce

  return (
    <div style={{
      width: 120,
      height: 120,
      backgroundColor: '#6366f1',
      borderRadius: '50%',
      ...scaleStyle,
    }} />
  );
};
```

## Variants

```tsx
// Snappy no-bounce (default)
withScaleIn(frame, fps, {damping: 200})

// Playful bounce
withScaleIn(frame, fps, {damping: 8, stiffness: 300})

// Delayed by 20 frames
withScaleIn(frame, fps, {delay: 20, damping: 200})
```
