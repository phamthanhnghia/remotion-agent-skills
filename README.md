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
npx skills add phamthanhnghia/remotion-agent-skills --full-depth
```

Alternatively, clone repository skills into your local project's `.agents/skills` directory:

```bash
git clone https://github.com/phamthanhnghia/remotion-agent-skills.git .agents/skills
```

## Structure

```
.agents/skills/
├── remotion/                   # Entry point — read first for any Remotion request
│   ├── SKILL.md
│   └── references/
│       ├── intent-interview.md
│       ├── capability-menu.md
│       ├── skill-lifecycle.md
│       ├── workflow-catalog.md
│       └── routes/             # One file per workflow route
│           ├── remotion-explainer.md
│           ├── remotion-product-launch.md
│           ├── remotion-motion-graphics.md
│           ├── remotion-slideshow.md
│           ├── remotion-music-video.md
│           ├── remotion-captions.md
│           ├── remotion-overlay.md
│           ├── remotion-data-viz.md
│           ├── remotion-general.md
│           └── hyperframes-to-remotion.md
│
├── remotion-core/              # Composition contract — read before writing code
│   ├── SKILL.md
│   └── references/
│       ├── minimal-composition.md
│       ├── determinism-rules.md
│       ├── timing-api.md
│       ├── sequence-and-series.md
│       └── calculate-metadata.md
│
├── remotion-animation/         # All motion knowledge
│   ├── SKILL.md
│   ├── rules-index.md
│   ├── blueprints-index.md
│   ├── rules/
│   │   ├── fade-up.md
│   │   ├── scale-in.md
│   │   └── typewriter.md
│   ├── blueprints/
│   │   ├── brand-reveal.md
│   │   └── stat-hero.md
│   ├── adapters/
│   │   ├── spring.md
│   │   ├── interpolate.md
│   │   ├── lottie.md
│   │   └── three.md
│   └── transitions/
│       ├── overview.md
│       └── catalog.md
│
├── remotion-keyframes/         # Precise frame-level animation
│   ├── SKILL.md
│   └── references/
│
├── remotion-cli/               # CLI commands — render, studio, lambda
│   ├── SKILL.md
│   └── references/
│       ├── render-options.md
│       └── lambda.md
│
├── remotion-creative/          # Design, typography, narration
│   ├── SKILL.md
│   └── references/
│       ├── house-style.md
│       └── typography.md
│
├── remotion-media/             # Assets — audio, video, images, fonts, captions
│   ├── SKILL.md
│   └── references/
│       ├── media-elements.md
│       └── captions.md
│
└── remotion-data-viz/          # Charts, counters, infographics
    ├── SKILL.md
    └── references/
        └── chart-patterns.md
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
