# Route: remotion-data-viz

- **Input:** A dataset (CSV, JSON, or tabular data), a data story brief, or a description of what the data should communicate.
- **Output:** A data-driven Remotion composition with animated charts, counters, progress bars, or infographics where numbers tell the story.
- **Triggers:** "animate my data", "make a chart video", "data visualization", "animated infographic", "bar chart race", "animated stats".

## Contract

- Requires: `data` (file or inline), `message` (what the data communicates), `chart_type` (bar / line / counter / pie / custom), `destination`
- Optional: `length`, `style_preset`, `brand_colors`

## Interview

1. **What is the data and what story should it tell?** (must-have)
2. **Chart type?** (bar / line chart / counter / pie / donut / bar race — recommend bar for comparisons, counter for single numbers, line for trends)
3. **Platform?** (16:9 / 9:16 / 1:1)
4. **Length?** (recommend 15–30 s per data point or counter)
5. Run-shape questions: (a) storyboard? (b) automation or companion?
