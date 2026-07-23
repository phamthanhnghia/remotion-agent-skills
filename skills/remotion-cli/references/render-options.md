# Render Options Reference

## Local render

```bash
# Basic render
npx remotion render <entry-file> <composition-id> <output-file>

# Example
npx remotion render src/index.ts MyComposition out/video.mp4
```

## Common flags

| Flag | Effect | Default |
| --- | --- | --- |
| `--codec` | Output codec: `h264`, `h265`, `vp8`, `vp9`, `prores`, `gif` | `h264` |
| `--crf` | Quality (lower = better): 0–51 for H.264 | `18` |
| `--fps` | Override FPS | Composition FPS |
| `--frames` | Render only a range: `0-60`, `100` | All frames |
| `--concurrency` | Browser instances in parallel | CPU count / 2 |
| `--width` / `--height` | Override composition dimensions | Composition size |
| `--props` | JSON string or file path for input props | `{}` |
| `--output` | Output file path | `out/` directory |
| `--quality` | JPEG quality for frames: 0–100 | `80` |
| `--log` | Log level: `error`, `warn`, `info`, `verbose` | `info` |
| `--overwrite` | Overwrite existing output | `false` |

## Codec guidance

| Use case | Codec | CRF |
| --- | --- | --- |
| Upload to YouTube / social | `h264` | `18` |
| High quality archive | `h264` | `10` |
| Transparency needed | `prores` with `4444` profile | — |
| Small file, browser play | `vp8` or `vp9` | `28` |
| Animated GIF | `gif` | — |
| Apple ecosystem | `prores` | — |

## Still image render

```bash
# Render one frame as PNG
npx remotion still src/index.ts MyComposition out/frame.png --frame=30

# Render one frame as JPEG
npx remotion still src/index.ts MyComposition out/frame.jpg --frame=30 --quality=90
```

## remotion.config.ts

```ts
import {Config} from '@remotion/cli/config';

Config.setVideoImageFormat('jpeg');  // faster renders
Config.setOverwriteOutput(true);
Config.setConcurrency(4);            // parallel browser instances
Config.setCodec('h264');
Config.setCrf(18);
Config.setOutputLocation('out/video.mp4');
```

## Verify output

Always verify after rendering:

```bash
# Check file exists and is non-empty
test -s out/video.mp4 && echo "OK" || echo "EMPTY"

# Check duration and format
ffprobe -v error -show_format -show_streams out/video.mp4

# Quick preview
ffplay out/video.mp4
```
