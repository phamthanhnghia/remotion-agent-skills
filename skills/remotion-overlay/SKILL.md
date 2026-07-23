---
name: remotion-overlay
description: >
  Graphic overlays workflow for Remotion: add designed info cards, badges, and graphics over existing footage.
  Use for video annotation, lower thirds over footage, callouts, and graphic enhancements.
---

# Remotion Overlay Workflow

Overlay animated graphic cards, lower thirds, callout badges, and info popups onto existing background video.

## Contract & Prerequisites

- **Input:** Existing video file + list of overlay graphics and timestamp schedules.
- **Output:** Remotion composition rendering background video via `<OffthreadVideo>` with layered React graphics.
- **Triggers:** "add graphic overlays", "annotate video with cards", "lower thirds over video".

## Composition Structure

- `<AbsoluteFill>` containing `<OffthreadVideo src={staticFile('input.mp4')} />`.
- Layered `<Sequence>` components rendering graphic cards at precise timestamps.
