# Captions Reference

## @remotion/captions

```bash
npm install @remotion/captions
```

### Word-timed captions

The `<Captions>` component from `@remotion/captions` renders animated word-by-word captions:

```tsx
import {Captions} from '@remotion/captions';
import {useCurrentFrame, useVideoConfig} from 'remotion';

type Subtitle = {
  text: string;
  startMs: number;
  endMs: number;
};

const CaptionedVideo: React.FC<{subtitles: Subtitle[]}> = ({subtitles}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const currentMs = (frame / fps) * 1000;

  const currentSubtitle = subtitles.find(
    (s) => currentMs >= s.startMs && currentMs < s.endMs
  );

  return (
    <div
      style={{
        position: 'absolute',
        bottom: 80,
        left: 0,
        right: 0,
        textAlign: 'center',
        fontSize: 48,
        fontWeight: 700,
        color: 'white',
        textShadow: '0 2px 8px rgba(0,0,0,0.8)',
        padding: '0 100px',
      }}
    >
      {currentSubtitle?.text}
    </div>
  );
};
```

### Transcription workflow

1. Record or provide audio/video file in `public/`
2. Transcribe with `@remotion/media-utils` or an external service (e.g., Whisper via `openai`)
3. Convert transcript to word-timed array
4. Pass as `inputProps` or `staticFile` JSON

```tsx
// Example word-timed subtitle format
const subtitles = [
  {text: "Hello world", startMs: 0, endMs: 1200},
  {text: "This is Remotion", startMs: 1300, endMs: 2800},
];
```

### Styling presets

```tsx
// Bold word highlight style
const captionStyle = {
  fontSize: 52,
  fontWeight: 800,
  color: '#ffffff',
  backgroundColor: 'rgba(0,0,0,0.6)',
  padding: '8px 20px',
  borderRadius: 8,
  display: 'inline-block',
};

// Lower-third style
const lowerThirdStyle = {
  position: 'absolute' as const,
  bottom: 100,
  left: '50%',
  transform: 'translateX(-50%)',
  textAlign: 'center' as const,
  ...captionStyle,
};
```
