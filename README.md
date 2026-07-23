# Remotion Skills Suite

A complete agent skill set for building programmatic videos with [Remotion](https://www.remotion.dev/) — modeled on the HyperFrames skill architecture.

## 🎬 Product Demo Video

[![Remotion Skills Suite Demo Video](https://img.youtube.com/vi/wWoTdSrEvag/maxresdefault.jpg)](https://youtu.be/wWoTdSrEvag)

👉 **Watch Demo Video on YouTube**: [https://youtu.be/wWoTdSrEvag](https://youtu.be/wWoTdSrEvag)

### 💬 Prompt used to create this video:
```text
Tạo thư mục riêng trong thư mục hiện tại tạo video "Giới thiệu hệ điều hành Ubuntu" bằng tiếng anh .
Chỉ dựa vào @[.agents/skills] hiện tại , ưu tiên dùng pnpm
```

## 📦 Installation

Install the Remotion Skills Suite directly into your AI Agent / Cursor / Antigravity workspace with a single command:

```bash
# Non-interactive (install all skills automatically)
npx skills add phamthanhnghia/remotion-agent-skills --all -y

# Interactive / full depth
npx skills add phamthanhnghia/remotion-agent-skills --full-depth
```

Alternatively, clone repository skills into your local project's `.agents/skills` directory:

```bash
git clone https://github.com/phamthanhnghia/remotion-agent-skills.git .agents/skills
```

## 🧠 Structure (19 Load-on-Demand Agent Skills)

```
.agents/skills/
├── remotion/                   # Main Entry Point & Router
│   ├── SKILL.md
│   └── references/
│
├── remotion-core/              # Composition contract & determinism
├── remotion-animation/         # Spring physics, motion patterns & blueprints
├── remotion-keyframes/         # Precise frame timing & interpolate curves
├── remotion-cli/               # Remotion CLI, studio, render & lambda
├── remotion-creative/          # Design tokens, typography & house style
├── remotion-media/             # Audio, video, image assets & captions
├── remotion-data-viz/          # Dynamic SVG charts, counters & infographics
│
├── remotion-explainer/         # Explainer videos & concept walkthroughs
├── remotion-product-launch/    # Product launch & app promo visualizers
├── remotion-motion-graphics/   # Unnarrated short motion hits (<10s)
├── remotion-slideshow/         # Slide decks & presentation videos
├── remotion-music-video/       # Beat-synced visualizers & audio reactive
├── remotion-captions/          # Word-highlight subtitled footage
├── remotion-overlay/           # Graphic overlays over existing video
├── remotion-general/           # Custom & long narrative compositions (>3m)
├── remotion-social/            # 9:16 vertical TikTok / Reel / Shorts clips
├── remotion-podcast/           # Podcast audiograms & waveform visualizers
└── hyperframes-to-remotion/    # HyperFrames HTML to Remotion migration
```

## Key differences from HyperFrames

| Concept | HyperFrames | Remotion |
| --- | --- | --- |
| Output format | HTML file + runtime | React component |
| Time source | GSAP paused timeline, `data-*` attrs | `useCurrentFrame()` |
| Animation | GSAP `.to()`, `.from()`, `.fromTo()` | `spring()` + `interpolate()` |
| Clips/scenes | `class="clip"` + `data-start`/`data-end` | `<Sequence from durationInFrames>` |
| Sub-compositions | `data-composition-src` + `<template>` | React component import |
| Media | `<video>` / `<audio>` direct children | `<OffthreadVideo>` / `<Audio>` |
| Render | `npx hyperframes render` | `npx remotion render` |
| Preview | `npx hyperframes preview` | `npx remotion studio` |
| Cloud render | HeyGen cloud / AWS Lambda | AWS Lambda via `@remotion/lambda` |
| Fonts | Web fonts via `<link>` | `@remotion/google-fonts` |
| Determinism | No `Math.random`, `Date.now`, `repeat: -1` | No `Math.random`, `Date.now`, CSS transitions |

## Quick start

```bash
# New project
npx create-video@latest --blank my-video
cd my-video
npm install
npm run dev          # opens Studio on localhost:3000

# Render
npx remotion render src/index.ts MyComposition out/video.mp4 --concurrency=1
```
