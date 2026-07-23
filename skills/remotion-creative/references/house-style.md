# Creative: House Style

Default design decisions for Remotion compositions when no explicit design spec is provided.

## Palette defaults

Dark background first — video reads better on dark:

- Background: `#0f0f0f` (near-black, not pure black)
- Surface: `#1a1a2e` (deep navy for cards/panels)
- Accent: `#6366f1` (indigo — modern, tech-neutral)
- Accent 2: `#a855f7` (purple — secondary highlights)
- Text primary: `#f9fafb` (warm white)
- Text secondary: `#9ca3af` (muted gray)
- Success: `#10b981` (emerald for positive stats)
- Warning: `#f59e0b` (amber for emphasis)

Override when: the brief specifies brand colors, or the target platform is bright (e.g., LinkedIn 1:1 for professional content).

## Typography defaults

Load via `@remotion/google-fonts` — never use a `<link>` tag:

```tsx
import {loadFont as loadInter} from '@remotion/google-fonts/Inter';
import {loadFont as loadDMSans} from '@remotion/google-fonts/DMSans';

const {fontFamily: inter} = loadInter();  // headings and UI
const {fontFamily: dm} = loadDMSans();    // body and captions
```

**Scale for 1920×1080:**

| Use | Size | Weight |
| --- | --- | --- |
| Hero headline | 96–144px | 800 |
| Section title | 64–80px | 700 |
| Subtitle | 48px | 600 |
| Body text | 32–40px | 400 |
| Caption | 24–28px | 400 |
| Label/badge | 20–24px | 600 |

Scale by 0.56× for 9:16 vertical (1080×1920), 0.6× for 1:1 (1080×1080).

## Spacing

Base unit: 16px. Use multiples: 8, 16, 24, 32, 48, 64, 96.

Composition padding: 80px (horizontal), 60px (vertical) for 1920×1080.

## Motion defaults

- Entrance: `spring({frame, fps, config: {damping: 200}})` — no bounce, natural deceleration
- Exit: `interpolate` fade over 20 frames, `extrapolateLeft: 'clamp'`
- Stagger: 6–8 frames between items for lists, 10–12 for large elements

## Layout recipe

Every scene follows: **background layer → content layer → foreground accent layer**

1. `<AbsoluteFill>` with background color/gradient/video
2. Content div with `position: 'absolute'`, explicit `top/left/width/height` or `padding` from comp dimensions
3. Decorative elements (particles, gradients, lines) as `position: 'absolute'` with pointer-events: none

Never put the background on the composition root's `style.backgroundColor` alone — always use a full-bleed child. Remotion's per-frame capture can produce unexpected results with root-level backgrounds.

## Lazy defaults to question

Before accepting these defaults, check whether the brief implies:

- Brand colors → override palette
- Bright platform (LinkedIn, Facebook) → consider light background variant
- Fast-paced content → reduce `damping` to 8–12 for bouncier entrances
- Dense data → increase body text to 36px minimum for readability after compression
