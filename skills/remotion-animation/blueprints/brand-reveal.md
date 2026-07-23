# Blueprint: Brand Reveal

A 4-phase brand reveal: logo enters → tagline reveals → expands/holds → clean exit. Use for intros, outros, and logo stings.

## Phases

| Phase | Frames | What happens |
| --- | --- | --- |
| Enter | 0–30 | Logo scales from 0.8 to 1.0 with spring, opacity fades in |
| Reveal | 20–50 | Tagline slides up and fades in with stagger |
| Hold | 50–90 | All elements held at full opacity |
| Exit | 90–120 | Everything fades out together |

## TSX recipe

```tsx
import {AbsoluteFill, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';
import {loadFont} from '@remotion/google-fonts/Inter';

const {fontFamily} = loadFont();

type BrandRevealProps = {
  logoText: string;
  tagline: string;
  accentColor?: string;
};

export const BrandReveal: React.FC<BrandRevealProps> = ({
  logoText,
  tagline,
  accentColor = '#6366f1',
}) => {
  const frame = useCurrentFrame();
  const {fps, durationInFrames} = useVideoConfig();

  // Phase 1: Logo enter (spring scale + fade in)
  const logoScale = spring({
    frame,
    fps,
    from: 0.8,
    to: 1,
    config: {damping: 200},
    durationInFrames: 30,
  });
  const logoOpacity = interpolate(frame, [0, 20], [0, 1], {extrapolateRight: 'clamp'});

  // Phase 2: Tagline reveal (delayed fade up)
  const taglineOpacity = interpolate(frame, [20, 40], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const taglineY = interpolate(frame, [20, 40], [20, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});

  // Phase 4: Exit fade (last 30 frames)
  const exitOpacity = interpolate(
    frame,
    [durationInFrames - 30, durationInFrames],
    [1, 0],
    {extrapolateLeft: 'clamp'}
  );

  const globalOpacity = Math.min(logoOpacity, exitOpacity);

  return (
    <AbsoluteFill
      style={{
        backgroundColor: '#0f0f0f',
        justifyContent: 'center',
        alignItems: 'center',
        flexDirection: 'column',
        gap: 24,
        opacity: exitOpacity,
      }}
    >
      {/* Logo */}
      <div
        style={{
          fontFamily,
          fontSize: 96,
          fontWeight: 800,
          color: '#ffffff',
          opacity: logoOpacity,
          transform: `scale(${logoScale})`,
          letterSpacing: '-0.02em',
        }}
      >
        {logoText}
      </div>

      {/* Accent line */}
      <div
        style={{
          width: interpolate(frame, [15, 40], [0, 200], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}),
          height: 3,
          backgroundColor: accentColor,
          borderRadius: 2,
        }}
      />

      {/* Tagline */}
      <div
        style={{
          fontFamily,
          fontSize: 36,
          fontWeight: 400,
          color: '#9ca3af',
          opacity: taglineOpacity,
          transform: `translateY(${taglineY}px)`,
          letterSpacing: '0.08em',
          textTransform: 'uppercase',
        }}
      >
        {tagline}
      </div>
    </AbsoluteFill>
  );
};
```

## Registration (Root.tsx)

```tsx
<Composition
  id="BrandReveal"
  component={BrandReveal}
  durationInFrames={120}
  fps={30}
  width={1920}
  height={1080}
  defaultProps={{
    logoText: 'ACME',
    tagline: 'Build the future',
    accentColor: '#6366f1',
  }}
/>
```
