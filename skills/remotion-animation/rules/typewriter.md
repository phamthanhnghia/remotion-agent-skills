# Rule: typewriter

Text appears character by character as if being typed. Use for code snippets, headlines that build drama, or captions.

## Recipe

```tsx
import {interpolate, useCurrentFrame} from 'remotion';

type TypewriterProps = {
  text: string;
  startFrame?: number;
  charsPerSecond?: number; // at 30 fps; default 15 chars/sec
  fps?: number;
};

const Typewriter: React.FC<TypewriterProps> = ({
  text,
  startFrame = 0,
  charsPerSecond = 15,
  fps = 30,
}) => {
  const frame = useCurrentFrame();
  const framesPerChar = fps / charsPerSecond;
  const adjustedFrame = Math.max(0, frame - startFrame);

  const charsToShow = Math.min(
    text.length,
    Math.floor(adjustedFrame / framesPerChar)
  );

  const visible = text.slice(0, charsToShow);
  const hasMoreChars = charsToShow < text.length;

  return (
    <span>
      {visible}
      {/* Optional blinking cursor */}
      {hasMoreChars && (
        <span style={{
          opacity: Math.floor((frame % 30) / 15) === 0 ? 1 : 0,
          color: '#6366f1',
        }}>|</span>
      )}
    </span>
  );
};

// Usage
<Typewriter
  text="Hello, World!"
  startFrame={20}
  charsPerSecond={12}
/>
```

## Word-by-word variant (smoother for long text)

```tsx
const WordByWord: React.FC<{text: string; fps: number}> = ({text, fps}) => {
  const frame = useCurrentFrame();
  const words = text.split(' ');
  const FRAMES_PER_WORD = 6;

  return (
    <div style={{display: 'flex', flexWrap: 'wrap', gap: '0.25em'}}>
      {words.map((word, i) => {
        const wordFrame = Math.max(0, frame - i * FRAMES_PER_WORD);
        const wordOpacity = Math.min(1, wordFrame / 4);
        return (
          <span key={i} style={{opacity: wordOpacity, display: 'inline-block'}}>
            {word}
          </span>
        );
      })}
    </div>
  );
};
```
