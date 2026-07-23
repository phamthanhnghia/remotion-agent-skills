# Three.js (R3F) Adapter Reference

## @react-three/fiber in Remotion

```bash
npm install @react-three/fiber three
npm install @types/three  # for TypeScript
```

Use React Three Fiber (R3F) to embed 3D scenes inside Remotion compositions. All 3D animation must be driven by `useCurrentFrame()` — not by Three.js AnimationMixer's internal clock.

## Basic 3D scene

```tsx
import {Canvas} from '@react-three/fiber';
import {useCurrentFrame, useVideoConfig} from 'remotion';
import {useRef} from 'react';

const RotatingBox: React.FC = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const timeInSeconds = frame / fps;

  return (
    <mesh rotation={[0, timeInSeconds * Math.PI * 0.5, 0]}>
      <boxGeometry args={[2, 2, 2]} />
      <meshStandardMaterial color="#6366f1" />
    </mesh>
  );
};

export const ThreeScene: React.FC = () => (
  <Canvas
    style={{width: 1920, height: 1080}}
    camera={{position: [0, 0, 6], fov: 60}}
    gl={{preserveDrawingBuffer: true}} // required for Remotion frame capture
  >
    <ambientLight intensity={0.4} />
    <directionalLight position={[5, 5, 5]} intensity={1} />
    <RotatingBox />
  </Canvas>
);
```

> **Critical:** Always set `gl={{preserveDrawingBuffer: true}}` on `<Canvas>`. Without it, Remotion's frame capture reads an empty buffer.

## Frame-driven AnimationMixer

For glTF/GLB models with pre-baked animations:

```tsx
import {useFrame} from '@react-three/fiber';
import {useGLTF, useAnimations} from '@react-three/drei';
import {useCurrentFrame, useVideoConfig} from 'remotion';

const AnimatedModel: React.FC = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const {scene, animations} = useGLTF(staticFile('model.glb'));
  const {actions, mixer} = useAnimations(animations, scene);

  // Set mixer time based on Remotion frame — never autoplay
  useFrame(() => {
    mixer.setTime(frame / fps);
  });

  return <primitive object={scene} />;
};
```

## Limitations

- R3F requires a WebGL context — verify headless Chrome has WebGL enabled (`--enable-webgl`)
- Complex 3D scenes with many objects significantly increase render time per frame
- Post-processing effects (bloom, SSAO) may not render correctly in headless mode — test thoroughly
- Use `--concurrency=1` when debugging 3D render output
