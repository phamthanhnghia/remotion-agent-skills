---
name: remotion
description: Mandatory entry point. Read this first for any request to make, create, edit, animate, or render a video, animation, or motion graphic using Remotion.
---

# Remotion entry point

Remotion **renders video from React** — a composition is a React component that receives `frame` and `fps` as context, computes layout and style deterministically from those values, and is captured frame-by-frame by a headless browser. The full authoring contract lives in `/remotion-core`; read it before writing composition code.

## 1. Start from project state

Apply the first matching row; do not evaluate lower state rows:

| State | Action |
| --- | --- |
| Explicit migration of existing HyperFrames HTML to Remotion | Read `references/routes/hyperframes-to-remotion.md`, then route directly to that workflow. Skip the intent layer. |
| Specific operation on an existing Remotion project: inspect, diagnose, validate, preview, render, or deploy | Perform only that operation. Skip intent and workflow routing; load `/remotion-cli` and any required domain skills. |
| Specific edit to an existing project | Make the edit. Do not run the intent layer. |
| `BRIEF.md` exists | Read `workflow` and `flow`. Execute that workflow. Ask no brief questions. |
| No brief, but `remotion.config.ts` or `src/Root.tsx` exists | Resume from project files. Infer the owning workflow from existing compositions. If it cannot be determined uniquely, ask one routing-only question; do not run the intent interview. |
| Fresh creation | Run the intent layer — `references/intent-interview.md` — then route once using § 2 table. |

If a fresh request does not identify the subject or input, ask what the video is about before routing. Check preferences before asking anything (`references/intent-interview.md`, step 1). A `figma.com` input changes intake, not routing.

### Keep the project Remotion version current

A scaffolded project pins `remotion@<version>` in its `package.json`. When resuming a project, probe once before the first render-affecting command:

```bash
npx remotion upgrade --dry-run
```

The probe is read-only and reports available upgrades. When the project is behind, apply with `npx remotion upgrade`, then verify with `npx remotion studio` that compositions still load. Name the old and new version in the run summary. If any composition breaks after upgrade, revert `package.json` and continue on the pinned version.

## 2. Route fresh creation

Use the first matching row. Match the requested **deliverable**, not a word or file type mentioned in passing.

| Priority | Request | Workflow |
| --- | --- | --- |
| 1 | Explicitly migrate an existing HyperFrames HTML composition | `/hyperframes-to-remotion` |
| 2 | Author a presentation, pitch deck, or slide-by-slide deck | `/remotion-slideshow` |
| 3 | Add captions or subtitles to existing talking-head footage | `/remotion-captions` |
| 4 | Add designed graphic overlays to existing footage without changing it | `/remotion-overlay` |
| 5 | Build a beat-synced video from a music track, with no narration | `/remotion-music-video` |
| 6 | Create a short, unnarrated, motion-first unit under 10 s | `/remotion-motion-graphics` |
| 7 | Visualize data — charts, counters, graphs, infographics in motion | `/remotion-data-viz` |
| 8 | Market or showcase a website, product, or app | `/remotion-product-launch` |
| 9 | Explain a topic, concept, or article with invented visuals | `/remotion-explainer` |
| 10 | Any other custom video or composition | `/remotion-general` |

Before finalizing the route, read `references/routes/<workflow>.md`. If the candidate does not satisfy its contract, continue routing. Read only the matched route file.

### Resolve common ambiguities

- A short animated title, logo sting, stat hit, or standalone lower-third is `/remotion-motion-graphics` when unnarrated and motion is the message.
- Existing footage with captions routes to `/remotion-captions`; footage with designed info cards routes to `/remotion-overlay`.
- A music file selects `/remotion-music-video` only when its beat grid drives the piece.
- Data charts are `/remotion-data-viz` when data is the primary message.
- Specialized narrative workflows support up to ~3 minutes; route longer pieces to `/remotion-general`.

## 3. Route once, then leave

For fresh creation the intent layer (`references/intent-interview.md`) ends by writing `BRIEF.md`. The brief is the only routing artifact the workflow reads; nothing later re-opens this skill or the interview.

## 4. Scaffold the project

```bash
# Interactive scaffold (recommended for first-time)
npx create-video@latest

# Non-interactive blank scaffold
npx create-video@latest --blank ./

# Start Studio dev server
npm run dev
```

Read the matched workflow SKILL.md before proceeding with authoring.

## 5. Load domain skills on demand

| Need | Skill |
| --- | --- |
| Composition structure, `useCurrentFrame`, `interpolate`, `spring`, `Sequence`, timing contract | `/remotion-core` |
| Motion rules, animation patterns, spring physics, interpolate recipes, easing | `/remotion-animation` |
| `spring`, `interpolate`, timing curves, keyframe stagger patterns | `/remotion-keyframes` |
| Design specs, palette, typography, narration, beat planning | `/remotion-creative` |
| Images, audio, video assets, captions, color grades, TTS | `/remotion-media` |
| CLI: `render`, `studio`, `lambda`, `bundle`, `upgrade`, `still` | `/remotion-cli` |
| Data charts, D3, recharts, custom SVG animations | `/remotion-data-viz` |

Domain skills never take ownership of the end-to-end deliverable. Load only what the active workflow needs.
