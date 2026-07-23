---
name: remotion-music-video
description: >
  Beat-synced music video workflow for Remotion: sync visuals to an audio music track.
  Use for music visualizers, beat-driven promos, audio spectrum animations, and song promos.
---

# Remotion Music Video Workflow

Build audio-reactive, beat-synced visualizer videos driven by a music track using Remotion.

## Contract & Prerequisites

- **Input:** Music audio file (`.mp3` / `.wav`), artist name, track title, and aesthetic preference.
- **Output:** Beat-synced visualizer or music video composition with audio-reactive elements.
- **Triggers:** "music visualizer", "make a video for this song", "beat synced video".

## Audio Reactivity Pipeline

1. Pre-extract audio data using `@remotion/media-utils`.
2. Map frequency bands (bass, mid, treble) to visual properties (scale, opacity, rotation, color glow).
3. Combine `<Audio>` with synchronized visual scenes.
