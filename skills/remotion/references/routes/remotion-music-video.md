# Route: remotion-music-video

- **Input:** A music track (local file or description) and a brief for the visual concept.
- **Output:** A beat-synced Remotion composition where cuts and visual motion align with the music's beat grid.
- **Triggers:** "beat-sync video", "music video", "make a video to this track", "visualize my music".

## Contract

- Requires: `music_file` or `music_description`, `message` (visual concept), `destination`
- Optional: `aspect`, `length` (defaults to track length), `style_preset`

## Interview

1. **What is the track?** (file path, description, or genre/mood — must-have)
2. **What is the visual concept?** (abstract motion / typography / product shots — must-have if unformed)
3. **Target platform?** (16:9 / 9:16 / 1:1)
4. Run-shape questions: (a) storyboard? (b) automation or companion?

After music file is provided, analyze the beat grid with `@remotion/media-utils` before routing to the workflow.
