# Remotion Data Viz: Chart Patterns Reference

Complete TSX patterns for common animated data visualization components.

## Animated Progress Bar

```tsx
import {interpolate, useCurrentFrame, useVideoConfig} from 'remotion';

type ProgressBarProps = {
  value: number;   // 0–100
  label: string;
  color?: string;
};

export const ProgressBar: React.FC<ProgressBarProps> = ({value, label, color = '#6366f1'}) => {
  const frame = useCurrentFrame();
  const {durationInFrames} = useVideoConfig();

  const progress = interpolate(frame, [0, durationInFrames * 0.7], [0, value / 100], {
    extrapolateRight: 'clamp',
    easing: (t) => 1 - Math.pow(1 - t, 3),
  });

  return (
    <div style={{marginBottom: 16}}>
      <div style={{display: 'flex', justifyContent: 'space-between', marginBottom: 8, color: '#9ca3af', fontSize: 24}}>
        <span>{label}</span>
        <span style={{color: '#ffffff', fontVariantNumeric: 'tabular-nums'}}>{Math.round(progress * 100)}%</span>
      </div>
      <div style={{height: 20, backgroundColor: '#1e1e2e', borderRadius: 10}}>
        <div style={{
          height: '100%',
          width: `${progress * 100}%`,
          backgroundColor: color,
          borderRadius: 10,
          transition: 'none',
        }} />
      </div>
    </div>
  );
};
```

## Bar Race (sorted bars competing)

```tsx
type RacingBarsProps = {
  data: {label: string; value: number; color: string}[];
  maxVisible?: number;
};

export const RacingBars: React.FC<RacingBarsProps> = ({data, maxVisible = 5}) => {
  const frame = useCurrentFrame();
  const {durationInFrames} = useVideoConfig();

  const progress = interpolate(frame, [0, durationInFrames * 0.85], [0, 1], {
    extrapolateRight: 'clamp',
    easing: (t) => t < 0.5 ? 2 * t * t : -1 + (4 - 2 * t) * t,
  });

  const computed = data
    .map((d) => ({...d, currentValue: d.value * progress}))
    .sort((a, b) => b.currentValue - a.currentValue)
    .slice(0, maxVisible);

  const maxValue = Math.max(...computed.map((d) => d.currentValue)) || 1;

  return (
    <div style={{display: 'flex', flexDirection: 'column', gap: 16, padding: 60}}>
      {computed.map((item, i) => (
        <div key={item.label} style={{display: 'flex', alignItems: 'center', gap: 20}}>
          <div style={{width: 200, fontSize: 24, color: '#9ca3af', textAlign: 'right', flexShrink: 0}}>
            {item.label}
          </div>
          <div style={{flex: 1, height: 52, backgroundColor: '#1e1e2e', borderRadius: 8}}>
            <div style={{
              height: '100%',
              width: `${(item.currentValue / maxValue) * 100}%`,
              backgroundColor: item.color,
              borderRadius: 8,
              display: 'flex',
              alignItems: 'center',
              justifyContent: 'flex-end',
              paddingRight: 12,
            }}>
              <span style={{color: 'white', fontWeight: 700, fontSize: 22, fontVariantNumeric: 'tabular-nums'}}>
                {Math.round(item.currentValue).toLocaleString()}
              </span>
            </div>
          </div>
        </div>
      ))}
    </div>
  );
};
```

## Donut Chart

```tsx
type DonutProps = {
  segments: {value: number; color: string; label: string}[];
  size?: number;
};

export const DonutChart: React.FC<DonutProps> = ({segments, size = 400}) => {
  const frame = useCurrentFrame();
  const {durationInFrames} = useVideoConfig();

  const progress = interpolate(frame, [0, durationInFrames * 0.75], [0, 1], {
    extrapolateRight: 'clamp',
    easing: (t) => 1 - Math.pow(1 - t, 2),
  });

  const cx = size / 2;
  const cy = size / 2;
  const outerR = size * 0.42;
  const innerR = size * 0.25;
  const total = segments.reduce((s, seg) => s + seg.value, 0);

  const paths: React.ReactNode[] = [];
  let cumAngle = -Math.PI / 2;

  for (const seg of segments) {
    const segAngle = (seg.value / total) * Math.PI * 2 * progress;
    const startA = cumAngle;
    const endA = cumAngle + segAngle;
    cumAngle = endA;

    const x1o = cx + outerR * Math.cos(startA), y1o = cy + outerR * Math.sin(startA);
    const x2o = cx + outerR * Math.cos(endA), y2o = cy + outerR * Math.sin(endA);
    const x1i = cx + innerR * Math.cos(endA), y1i = cy + innerR * Math.sin(endA);
    const x2i = cx + innerR * Math.cos(startA), y2i = cy + innerR * Math.sin(startA);
    const large = segAngle > Math.PI ? 1 : 0;

    paths.push(
      <path
        key={seg.label}
        d={`M${x1o},${y1o} A${outerR},${outerR} 0 ${large},1 ${x2o},${y2o} L${x1i},${y1i} A${innerR},${innerR} 0 ${large},0 ${x2i},${y2i} Z`}
        fill={seg.color}
      />
    );
  }

  return <svg width={size} height={size}>{paths}</svg>;
};
```
