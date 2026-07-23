# Typography Reference

## Google Fonts in Remotion

Always use `@remotion/google-fonts` — never `<link rel="stylesheet">` or `@import url()`:

```bash
npm install @remotion/google-fonts
```

```tsx
// Import the specific font variant
import {loadFont} from '@remotion/google-fonts/Inter';
import {loadFont as loadMono} from '@remotion/google-fonts/JetBrainsMono';

const {fontFamily} = loadFont();
const {fontFamily: monoFamily} = loadMono();
```

The font loads before the first frame. No `delayRender` needed for Google Fonts.

## Recommended pairings

| Pairing | Heading | Body | Use case |
| --- | --- | --- | --- |
| Modern tech | Inter | Inter | SaaS, product, explainer |
| Editorial | Playfair Display | Lato | Documentary, luxury brand |
| Bold impact | Montserrat | Open Sans | Social media, promo |
| Code/developer | JetBrains Mono | Inter | Dev tool, PR explainer |
| Geometric | Outfit | Outfit | Minimal, data viz |

## Text rendering rules for video

1. **Never use system fonts** — they differ per OS and render inconsistently
2. **Minimum 24px for captions** at 1080p — goes to 36px after compression artifacts
3. **`fontVariantNumeric: 'tabular-nums'`** for counters — prevents layout shift during animation
4. **`whiteSpace: 'nowrap'`** for single-line headings — prevent unexpected wrapping
5. **`letterSpacing`** for all-caps labels: `0.08em` to `0.12em`
6. **`lineHeight: 1.2`** for multi-line headings, `1.5` for body text
7. **Do not use `<br>` for line breaks** — use multiple `<div>` or `<p>` elements; `<br>` can cause inconsistent rendering in headless Chrome

## Stroke / outline text

```tsx
// Text with outline (for readability over busy backgrounds)
<div
  style={{
    color: 'white',
    textShadow: '2px 2px 0 #000, -2px -2px 0 #000, 2px -2px 0 #000, -2px 2px 0 #000',
    WebkitTextStroke: '1px rgba(0,0,0,0.5)',
  }}
>
  Text on video
</div>
```
