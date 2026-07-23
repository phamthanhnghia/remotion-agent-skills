# Route: remotion-captions

- **Input:** Existing footage (talking-head, podcast, interview) with or without a transcript.
- **Output:** A Remotion composition that plays the original footage and overlays word-timed animated captions using `@remotion/captions`.
- **Triggers:** "add captions to my video", "subtitle my clip", "add subtitles", "caption this footage".

## Contract

- Requires: `footage_file`, `language` (for transcription if no transcript provided)
- Optional: `transcript_file`, `caption_style` (default / bold / outline), `destination`

## Interview

Not served by the full intent layer — this is a media operation, not a creative brief.

Ask only:
1. **Footage file path?** (must-have)
2. **Existing transcript?** (yes: provide path / no: I will transcribe)
3. **Caption style?** (default white subtitle / bold word highlight / outline for readability — recommend bold highlight)
