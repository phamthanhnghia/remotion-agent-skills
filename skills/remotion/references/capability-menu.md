# Capability menu — what Remotion can bring to a video

One list, three readers. The **pitch round** speaks it before anyone reads it as a menu — each pitch names the capability or two its concept rides. The **intent layer** recommends from it — one or two rows the confirmed concept specifically calls for. **`/remotion-general` in companion mode** uses it as its trigger list and upgrade channel.

| Capability | Say it to the user as… | Home → entry → what you get |
| --- | --- | --- |
| **Spring animations** — natural physics-based entrances, exits, pops, bounces driven by `spring()` | "motion that feels alive — elements spring into place instead of sliding linearly" | `/remotion-animation` → `adapters/spring.md` → spring configs |
| **Interpolate sequences** — multi-stop value transitions, opacity fades, position lerps, color shifts | "smooth timed transitions between any visual states" | `/remotion-animation` → `adapters/interpolate.md` → value mappings |
| **Stagger patterns** — arrays of elements entering with ordered delays for a cascade feel | "elements enter one after another in a satisfying wave" | `/remotion-keyframes` → stagger pattern section |
| **Text animations** — word-by-word, char-by-char, line reveals with spring physics | "words reveal themselves as the narration plays" | `/remotion-animation` → `adapters/text-animation.md` |
| **Data visualizations** — animated counters, bar charts, line charts, pie reveals, bar races | "numbers and charts animate in to make data memorable" | `/remotion-data-viz` → chart patterns |
| **Voice, music, SFX** — TTS narration, background music, sound effects synced to the timeline | "narration in a voice you pick, background music, sound effects timed to cuts" | `/remotion-media` → media elements |
| **Beat analysis & audio-reactive motion** — cuts land on the beat, elements pulse with the music | "if there's music, I can cut to its beat and make elements react to it" | `/remotion-media` → `references/audio-reactive.md` |
| **Video footage as layers** — `<OffthreadVideo>` composited with animated overlays | "your own clip plays while designed titles and callouts appear over it" | `/remotion-media` → `references/media-elements.md` |
| **Captions & subtitles** — word-timed via `@remotion/captions`, styled to match the design | "accurate captions styled to match the video" | `/remotion-media` → `references/captions.md` |
| **Google Fonts** — `@remotion/google-fonts` loads any font server-side before the first frame | "professional typography with any Google Font, guaranteed for every frame" | `/remotion-media` → `references/fonts.md` |
| **AWS Lambda rendering** — distributed cloud rendering for fast turnaround on long videos | "renders in minutes instead of hours using cloud parallelism" | `/remotion-cli` → `references/lambda.md` |
| **Input props / calculateMetadata** — composition-level variables that drive duration and layout | "one composition, many different videos — pass different data to get different results" | `/remotion-core` → `references/calculate-metadata.md` |
| **Three.js / R3F** — 3D scenes, camera motion, shader-driven visuals in a Remotion composition | "fully 3D scenes rendered frame-perfectly into the video" | `/remotion-animation` → `adapters/three.md` |
| **Lottie** — After Effects animations via `@remotion/lottie`, frame-synchronized | "After Effects animations play in sync with the video timeline" | `/remotion-animation` → `adapters/lottie.md` |

## The design ask — have it, pick it, or leave it

- **They have one.** Brand guidelines, a `frame.md`, or a `design.md` — note the path in `BRIEF.md` § Assets; the workflow reads it as brand truth.
- **They don't, but the look matters.** Pick 2–3 visual style presets from `remotion-creative/references/visual-styles.md` and describe them; the user picks by description.
- **They don't care.** No further questions; the workflow's design step decides and says why.
