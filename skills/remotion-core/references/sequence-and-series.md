# Sequence and Series Reference

## Sequence

`<Sequence>` shifts the timeline for its children. Children see frame 0 when the parent is at `from`. Children are hidden (not rendered) outside their `[from, from + durationInFrames)` window.

```tsx
import {Sequence} from 'remotion';

// Child sees frames 0–59 while parent is at frames 30–89
<Sequence from={30} durationInFrames={60} name="IntroScene">
  <IntroScene />
</Sequence>
```

**Key props:**

| Prop | Type | Effect |
| --- | --- | --- |
| `from` | number | Frame when the sequence starts. Required. |
| `durationInFrames` | number | How many frames the sequence lasts. Omit to inherit parent duration. |
| `name` | string | Label shown in Studio timeline (optional but recommended) |
| `layout` | `'absolute-fill'` or `'none'` | Default `'absolute-fill'`: wraps in an `AbsoluteFill`. Use `'none'` for inline content. |

**Nesting:** Sequences can nest. The inner `from` is relative to the outer Sequence's local frame-0.

## Series

`<Series>` chains Sequences one after another, automatically computing `from` values.

```tsx
import {Series} from 'remotion';

<Series>
  <Series.Sequence durationInFrames={60} name="Intro">
    <IntroScene />
  </Series.Sequence>
  <Series.Sequence durationInFrames={90} name="Main">
    <MainScene />
  </Series.Sequence>
  <Series.Sequence durationInFrames={60} name="Outro">
    <OutroScene />
  </Series.Sequence>
</Series>
```

The second scene starts at frame 60, the third at frame 150. No manual `from` calculation needed.

**offset prop:** Add inter-scene gaps or overlaps:

```tsx
<Series.Sequence durationInFrames={90} offset={-15}> // overlaps previous by 15 frames
```

## Freeze

`<Freeze>` locks children to a specific frame, useful for thumbnails or still elements:

```tsx
import {Freeze} from 'remotion';

<Freeze frame={30}>
  <MyComponent /> {/* always renders as if at frame 30 */}
</Freeze>
```

## Loop

`<Loop>` repeats a child composition N times or indefinitely (for finite-duration children only — looping infinite children is banned):

```tsx
import {Loop} from 'remotion';

<Loop times={3} durationInFrames={30}>
  <PulseAnimation />
</Loop>
```
