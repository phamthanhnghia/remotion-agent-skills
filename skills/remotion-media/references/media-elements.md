# Media Elements Reference

## OffthreadVideo

Use `<OffthreadVideo>` for all video files in compositions that will be rendered. It renders each frame off the main thread, producing frame-perfect results.

```tsx
import {OffthreadVideo, staticFile} from 'remotion';

// Full-screen video background
<OffthreadVideo
  src={staticFile('footage.mp4')}
  style={{width: '100%', height: '100%', objectFit: 'cover'}}
/>

// With trim
<OffthreadVideo
  src={staticFile('footage.mp4')}
  startFrom={30}          // skip first 30 frames of the video file
  endAt={120}             // stop at frame 120 of the video file
  volume={0}              // mute (background video)
/>

// With volume ramp
<OffthreadVideo
  src={staticFile('footage.mp4')}
  volume={(f) => interpolate(f, [0, 30], [0, 1], {extrapolateRight: 'clamp'})}
/>
```

## Audio

```tsx
import {Audio, staticFile} from 'remotion';

// Background music
<Audio
  src={staticFile('bgm.mp3')}
  volume={0.3}
  loop                // loops for the full composition duration
/>

// Narration starting at frame 30
<Audio
  src={staticFile('narration.mp3')}
  startFrom={0}
  volume={1}
/>

// SFX triggered at a specific moment (wrap in <Sequence> for timing)
<Sequence from={45} durationInFrames={60}>
  <Audio src={staticFile('whoosh.mp3')} volume={0.8} />
</Sequence>
```

## Img

```tsx
import {Img, staticFile} from 'remotion';

// Static image
<Img
  src={staticFile('hero.jpg')}
  style={{width: '100%', height: '100%', objectFit: 'cover'}}
/>

// With animation
<Img
  src={staticFile('product.png')}
  style={{
    opacity: interpolate(frame, [0, 20], [0, 1], {extrapolateRight: 'clamp'}),
    transform: `scale(${spring({frame, fps, config: {damping: 200}})})`,
  }}
/>
```

## staticFile

All assets in `public/` must be referenced via `staticFile()`:

```tsx
import {staticFile} from 'remotion';

staticFile('video.mp4')         // public/video.mp4
staticFile('images/hero.jpg')   // public/images/hero.jpg
staticFile('fonts/custom.woff2') // public/fonts/custom.woff2
```

Do not use:
- Relative paths: `'./public/video.mp4'`
- Absolute paths: `'C:/Users/...'`
- CDN URLs that require network access at render time

## OffthreadVideo vs Video (quick reference)

| | OffthreadVideo | Video |
| --- | --- | --- |
| Frame accuracy | ✅ Frame-perfect | ❌ May drop frames |
| Rendering | Off main thread | In-browser playback |
| Use in render | ✅ Required | ❌ Not recommended |
| Use in Studio preview | ✅ Works | ✅ Simpler for dev |

Always use `<OffthreadVideo>` in production-render compositions.
