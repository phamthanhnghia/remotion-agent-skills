# Remotion Core References

## minimal-composition.md

The smallest renderable Remotion composition:

```bash
npx create-video@latest --blank my-video
cd my-video
npm install
npm run dev
```

Minimum `src/Root.tsx`:

```tsx
import {Composition} from 'remotion';
import {MyVideo} from './MyVideo';

export const RemotionRoot: React.FC = () => (
  <Composition
    id="MyVideo"
    component={MyVideo}
    durationInFrames={150}
    fps={30}
    width={1920}
    height={1080}
  />
);
```

Minimum `src/MyVideo.tsx`:

```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const MyVideo: React.FC = () => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{backgroundColor: '#0f0f0f', justifyContent: 'center', alignItems: 'center'}}>
      <h1 style={{color: 'white', fontSize: 72}}>Frame {frame}</h1>
    </AbsoluteFill>
  );
};
```
