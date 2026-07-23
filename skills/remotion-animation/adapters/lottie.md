# Lottie Adapter Reference

## @remotion/lottie

```bash
npm install @remotion/lottie
```

Lottie animations (After Effects exports) can be integrated into Remotion compositions. The `<Lottie>` component from `@remotion/lottie` synchronizes the Lottie animation to Remotion's `useCurrentFrame()`, making it seek-safe and frame-deterministic.

## Basic usage

```tsx
import {Lottie} from '@remotion/lottie';
import {staticFile, useVideoConfig, useCurrentFrame} from 'remotion';
import animationData from './public/animation.json';

export const LottieScene: React.FC = () => {
  const {fps} = useVideoConfig();
  const frame = useCurrentFrame();

  return (
    <Lottie
      animationData={animationData}
      playbackRate={1}
      style={{width: 400, height: 400}}
    />
  );
};
```

The `<Lottie>` component automatically maps Remotion frames to Lottie frames based on the animation's native FPS.

## Controlling playback

```tsx
// Start the Lottie animation only after frame 30
<Sequence from={30}>
  <Lottie
    animationData={animationData}
    loop={false}  // play once, don't loop
  />
</Sequence>

// Loop the animation a fixed number of times
<Lottie
  animationData={animationData}
  loop={3}       // loop 3 times then hold last frame
/>
```

## Loading from staticFile (public/ directory)

If the JSON is large, import it dynamically using `delayRender`/`continueRender`:

```tsx
import {delayRender, continueRender, staticFile} from 'remotion';
import {useState, useEffect} from 'react';

const LottieFromFile: React.FC = () => {
  const [animData, setAnimData] = useState(null);

  useEffect(() => {
    const handle = delayRender('Loading Lottie JSON');
    fetch(staticFile('animation.json'))
      .then((r) => r.json())
      .then((data) => {
        setAnimData(data);
        continueRender(handle);
      });
  }, []);

  if (!animData) return null;

  return <Lottie animationData={animData} />;
};
```

## Limitations

- Lottie animations that use expressions or external assets may not render correctly
- Test each Lottie file in Studio before committing to a render
- Very complex Lottie files can slow render time significantly
- `loop: true` is banned — always set an explicit finite loop count or `false`
