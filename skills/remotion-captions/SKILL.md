---
name: remotion-captions
description: >
  Captions and subtitles workflow for Remotion: add styled word-by-word captions to video footage.
  Use for talking head videos, shorts, podcasts, reels, and auto-captioned video clips.
---

# Remotion Captions Workflow

Add dynamic, word-by-word animated captions and subtitles to existing talking-head or voiceover footage.

## Contract & Prerequisites

- **Input:** Video footage or audio track + caption transcript (JSON/SRT/VTT).
- **Output:** Remotion composition with `<OffthreadVideo>` and word-highlight animated subtitles.
- **Triggers:** "add captions to video", "subtitle this clip", "create captioned reel".

## Features

- Word-level highlight timing with `spring()` bounce.
- Preset subtitle styles: TikTok / Hormozi style, clean modern lower-bar, YouTube style.
- Automatic text wrapping and safety margin calculations.
