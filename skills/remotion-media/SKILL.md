---
name: remotion-media
description: Agent Media OS for Remotion projects. Resolve BGM, SFX, images, fonts, brand logos, TTS voiceover, and color grades into frozen local files or paste-ready blocks.
---

# remotion-media

The media OS for Remotion: resolve · place · verify — every media type, one skill.

## The placement contract

All static assets live in the `public/` directory. Reference them exclusively via `staticFile()`:

```tsx
import {staticFile} from 'remotion';

// Images
<Img src={staticFile('hero.jpg')} />

// Audio
<Audio src={staticFile('background.mp3')} volume={0.4} />

// Video
<OffthreadVideo src={staticFile('footage.mp4')} />
```

**Never** use relative paths, absolute disk paths, or CDN URLs that require network access at render time (they will fail in offline renders or Lambda).

## Asset types and their tools

| Type | Tool / Package | Usage |
| --- | --- | --- |
| Background music | Resolved local file | `<Audio src={staticFile('bgm.mp3')} volume={0.3} />` |
| SFX | Resolved local file | `<Audio src={staticFile('click.mp3')} startFrom={30} />` |
| Images / backgrounds | Local file or `staticFile()` | `<Img src={staticFile('bg.jpg')} />` |
| Google Fonts | `@remotion/google-fonts` | `loadFont()` returns `{fontFamily}` |
| Brand logo (SVG) | Local SVG in public/ | `<Img src={staticFile('logo.svg')} />` |
| Voiceover / TTS | Local mp3 file | `<Audio src={staticFile('narration.mp3')} />` |
| Video footage | Local mp4 file | `<OffthreadVideo src={staticFile('clip.mp4')} />` |
| Captions / subtitles | `@remotion/captions` | Word-timed `<Captions>` component |

## Fonts

Always use `@remotion/google-fonts` for Google Fonts — never a `<link>` tag:

```bash
npm install @remotion/google-fonts
```

```tsx
import {loadFont} from '@remotion/google-fonts/Inter';

const {fontFamily} = loadFont();

const Text = () => (
  <div style={{fontFamily, fontWeight: 700}}>Hello</div>
);
```

For custom fonts, place the font file in `public/fonts/` and load via `FontFace` inside `calculateMetadata` or a `delayRender`/`continueRender` pair.

## delayRender — async asset loading

Use `delayRender` / `continueRender` from `remotion` when assets must be fetched or generated before the first frame renders:

```tsx
import {delayRender, continueRender} from 'remotion';

const handle = delayRender('Loading font');

fetch('/api/data')
  .then(r => r.json())
  .then(data => {
    setData(data);
    continueRender(handle);
  });
```

Do not use `delayRender` inside the render function — call it at module scope or in a `useEffect` that fires once.

## Audio / Video timing

| Prop | Effect |
| --- | --- |
| `startFrom` | Skip the first N frames of the asset |
| `endAt` | Stop playback at frame N of the asset |
| `volume` | 0–1 scalar or a function of frame |
| `playbackRate` | Speed multiplier |
| `loop` | Loop audio (deterministic: repeats based on composition duration) |

## OffthreadVideo vs Video

| | `<OffthreadVideo>` | `<Video>` |
| --- | --- | --- |
| Render accuracy | Frame-perfect (recommended) | May drop frames |
| Seek behavior | Rendered off main thread | Plays back in-browser |
| Use case | Final composition | Preview only |

**Always use `<OffthreadVideo>` for renders.** `<Video>` is acceptable in Studio preview only.

## Captions

Use `@remotion/captions` for word-timed captions:

```bash
npm install @remotion/captions
```

```tsx
import {Captions} from '@remotion/captions';

<Captions
  subtitles={wordTimedSubtitles}
  startFrom={0}
  windowStart={currentWordStart}
  windowEnd={currentWordEnd}
  style={{fontSize: 48, color: 'white'}}
/>
```

Transcription workflow → `references/captions.md`.

## Audio-reactive prep

Pre-analyze audio outside the render pipeline using `@remotion/media-utils`:

```tsx
import {getAudioData, visualizeAudio} from '@remotion/media-utils';

// In calculateMetadata (runs once before render):
const audioData = await getAudioData(staticFile('music.mp3'));
```

Full audio-reactive authoring → `remotion-creative/references/audio-reactive.md`.

## Be proactive — run a media opportunity pass

When you build or review a composition, do **one** grounded scan and then ask once:

| Signal detected | Offer |
| --- | --- |
| On-screen text with no voiceover | TTS voiceover option |
| Emoji or placeholder icon div | Resolve real SVG icon |
| Placeholder image or solid-color background | Suggest a real image from public/ |
| Hard cuts with no sound | Suggest transition SFX |
| Piece over ~10 s with no music bed | Suggest BGM track |

Rules: **grounded, not generic** (no signal → no suggestion); **propose the specific fix**; **once per project** (one consolidated ask).

## Where to look

| Task | Read |
| --- | --- |
| staticFile, public/ directory, asset paths | `references/static-files.md` |
| delayRender for async loading | `references/async-assets.md` |
| Font loading (@remotion/google-fonts, custom) | `references/fonts.md` |
| OffthreadVideo, Audio trim, volume curves | `references/media-elements.md` |
| Captions, transcription, @remotion/captions | `references/captions.md` |
| Audio-reactive motion, getAudioData | `references/audio-reactive.md` |
