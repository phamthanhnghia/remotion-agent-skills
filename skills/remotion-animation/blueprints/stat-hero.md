# Blueprint: Stat Hero

A 4-phase animated statistic: entrance → count-up → proof context → hold. Use for one or two key numbers.

## Phases

| Phase | Frames | What happens |
| --- | --- | --- |
| Enter | 0–20 | Label slides up and fades in |
| Count | 20–80 | Number counts from 0 to target with ease-out |
| Proof | 50–90 | Supporting context (subtitle, chart, trend) appears |
| Hold | 90–120 | All elements at full opacity |

## TSX recipe

```tsx
import {AbsoluteFill, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

type StatHeroProps = {
  value: number;
  label: string;
  prefix?: string;
  suffix?: string;
  context?: string;
  color?: string;
};

export const StatHero: React.FC<StatHeroProps> = ({
  value,
  label,
  prefix = '',
  suffix = '',
  context = '',
  color = '#6366f1',
}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();

  // Label entrance
  const labelOpacity = interpolate(frame, [0, 20], [0, 1], {extrapolateRight: 'clamp'});
  const labelY = interpolate(frame, [0, 20], [30, 0], {extrapolateRight: 'clamp'});

  // Count up
  const countProgress = interpolate(frame, [20, 80], [0, 1], {
    extrapolateLeft: 'clamp',
    extrapolateRight: 'clamp',
    easing: (t) => 1 - Math.pow(1 - t, 3), // ease-out cubic
  });
  const displayValue = Math.round(value * countProgress);

  // Number scale pop at arrival
  const scaleAtArrival = frame >= 79
    ? spring({frame: frame - 79, fps, config: {damping: 8, stiffness: 300}, durationInFrames: 20})
    : 1;
  const numScale = 1 + (scaleAtArrival - 1) * 0.08;

  // Context appearance
  const contextOpacity = interpolate(frame, [55, 80], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});

  return (
    <AbsoluteFill style={{backgroundColor: '#0f0f0f', justifyContent: 'center', alignItems: 'center', flexDirection: 'column', gap: 24}}>
      {/* Label */}
      <div style={{
        fontSize: 32,
        fontWeight: 600,
        color: '#9ca3af',
        textTransform: 'uppercase',
        letterSpacing: '0.12em',
        opacity: labelOpacity,
        transform: `translateY(${labelY}px)`,
      }}>
        {label}
      </div>

      {/* Number */}
      <div style={{
        fontSize: 160,
        fontWeight: 800,
        color: '#ffffff',
        lineHeight: 1,
        fontVariantNumeric: 'tabular-nums',
        transform: `scale(${numScale})`,
      }}>
        <span style={{color, fontSize: 80}}>{prefix}</span>
        {displayValue.toLocaleString()}
        <span style={{color, fontSize: 80}}>{suffix}</span>
      </div>

      {/* Context */}
      {context && (
        <div style={{
          fontSize: 28,
          color: '#6b7280',
          opacity: contextOpacity,
          maxWidth: 600,
          textAlign: 'center',
        }}>
          {context}
        </div>
      )}
    </AbsoluteFill>
  );
};
```
