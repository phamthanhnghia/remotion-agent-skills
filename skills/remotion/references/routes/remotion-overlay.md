# Route: remotion-overlay

- **Input:** Existing footage (talking-head, interview, podcast) without captions; the intent is to add graphic overlays (lower-thirds, callouts, titles, info cards) rather than plain captions.
- **Output:** A Remotion composition that plays the original footage and composites animated overlay components using `<OffthreadVideo>` plus `<Sequence>` timed overlays.
- **Triggers:** "add lower thirds", "add titles to my footage", "add info cards", "add name overlays", "design overlays for my talking-head video".

## Contract

- Requires: `footage_file`, `overlay_content` (what the overlays say), `destination`
- Optional: `style_preset`, `brand_colors`, `transcript_file` for timing

## Interview

1. **Footage file path?** (must-have)
2. **What should the overlays say?** (names, titles, callouts — must-have)
3. **Brand colors or style?** (optional — defer to workflow)
4. Run-shape question (b) only: automation or companion?
