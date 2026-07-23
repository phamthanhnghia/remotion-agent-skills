---
name: remotion-podcast
description: >
  Podcast visualizer workflow for Remotion: turn podcast audio into an engaging video clip with waveforms.
  Use for podcast audiograms, speaker cards, audio waveform visualizers, and talk show clips.
---

# Remotion Podcast Visualizer Workflow

Transform podcast audio snippets into visual audiogram videos with animated waveforms and speaker graphics.

## Contract & Prerequisites

- **Input:** Podcast audio file (`.mp3`/`.wav`), speaker photos/names, episode title, and transcript.
- **Output:** Remotion composition featuring dynamic audio waveforms, speaker badges, and animated captions.
- **Triggers:** "podcast audiogram", "make a video for podcast clip", "audio waveform video".

## Architecture

- Extract audio data using `@remotion/media-utils`.
- Render dynamic SVG waveform bars driven by audio frequencies.
- Integrate active speaker highlighted cards and animated subtitles.
