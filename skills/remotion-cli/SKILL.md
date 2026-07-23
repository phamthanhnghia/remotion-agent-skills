---
name: remotion-cli
description: Use the Remotion CLI development loop. Studio dev server, render compositions to MP4, deploy to Lambda, sites, functions, compositions list, and troubleshooting.
---

# Remotion CLI

Run commands as `npx remotion ...` or via package scripts if the project defines them. The CLI requires Node.js 18+ and for render operations a Chrome/Chromium installation (or use `npx remotion install-executables`).

## Development loop

1. **Scaffold:** `npx create-video@latest` or clone a template. For non-interactive: `npx create-video@latest --blank ./my-project`.
2. **Author:** write compositions using `/remotion-core`.
3. **Fast feedback while editing:** `npm run dev` opens Remotion Studio on `localhost:3000`.
4. **Check in Studio:** scrub the timeline, verify all frames render, check all compositions with `npx remotion compositions src/index.ts`.
5. **Render a draft:** use `--concurrency=1` first to detect non-determinism bugs.
6. **Preview with the user:** open Studio, hand the URL, ask whether to revise or render final.
7. **Render only after approval:** use high quality codec for delivery.
8. **Verify the output:** confirm the file exists, is non-empty, and has the expected duration via `ffprobe`.

```bash
# Dev server (fast live preview)
npm run dev
# OR
npx remotion studio src/index.ts

# List all registered compositions
npx remotion compositions src/index.ts

# Draft render (fast, lower quality)
npx remotion render src/index.ts MyComposition out/draft.mp4 --concurrency=1

# Final render (high quality)
npx remotion render src/index.ts MyComposition out/final.mp4 --codec=h264 --crf=18

# Verify output
ffprobe -v error -show_format -show_streams out/final.mp4
```

## Remotion Studio

| Surface | When | Purpose |
| --- | --- | --- |
| Studio (`npm run dev`) | During authoring | Live frame preview, timeline scrubbing, props editor |
| Studio Input Props panel | At any time | Edit composition props and see real-time results |
| Studio render button | After approval | Trigger local render from the browser |

Studio runs on `localhost:3000` by default. It is the primary review surface before any render.

## render command options

| Need | Command |
| --- | --- |
| Fast local iteration | `npx remotion render ... --concurrency=1 --quality=draft` |
| Final local delivery (H.264) | `npx remotion render ... --codec=h264 --crf=18` |
| Final local delivery (ProRes) | `npx remotion render ... --codec=prores --prores-profile=4444` |
| Still image from one frame | `npx remotion still ... --frame=30` |
| Override composition size | `npx remotion render ... --width=1920 --height=1080` |
| Override FPS | `npx remotion render ... --fps=60` |
| Override duration | `npx remotion render ... --frames=0-150` |
| Pass input props | `npx remotion render ... --props='{"title":"Hello"}'` |
| Props from JSON file | `npx remotion render ... --props=./props.json` |

## AWS Lambda rendering

```bash
# Install Lambda package
npm install @remotion/lambda

# Deploy Lambda function (run once per AWS account/region)
npx remotion lambda functions deploy --memory=3009 --timeout=120

# Create a site (upload bundle to S3)
npx remotion lambda sites create --site-name=my-video-site

# Render via Lambda (fast, parallel)
npx remotion lambda render my-video-site MyComposition --region=us-east-1

# Check render progress
npx remotion lambda renders progress <render-id> --region=us-east-1
```

> Read `references/lambda.md` before running any Lambda command. Lambda requires IAM roles, S3 bucket, and environment variables (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION`).

## Agent conventions

- Use `--concurrency=1` when debugging to surface non-determinism bugs
- Use `--log=verbose` when diagnosing render failures
- Never render merely because Studio looks correct — always wait for user approval
- Use `npx remotion compositions src/index.ts` to verify all compositions registered before rendering
- For batch renders, use the Node.js API (`renderMedia`, `renderFrames`) rather than looping CLI calls
- JSON output is available via `--log=json` on some commands

## remotion.config.ts

Place `remotion.config.ts` at the project root to set persistent defaults:

```ts
import {Config} from '@remotion/cli/config';

Config.setVideoImageFormat('jpeg');
Config.setOverwriteOutput(true);
Config.setConcurrency(4);
Config.setCodec('h264');
Config.setCrf(18);
```

## Read the matching reference before running a command

| Need | Reference |
| --- | --- |
| `studio`, `render`, `still`, codec options | `references/render-options.md` |
| `lambda functions`, `lambda sites`, `lambda render` | `references/lambda.md` |
| `bundle` for custom deployment | `references/bundle.md` |
| `upgrade`, `install-executables`, `doctor` | `references/upgrade-misc.md` |
| `compositions`, `--props`, `calculateMetadata` | `references/compositions.md` |

For composition variables and `calculateMetadata`, also read `/remotion-core` → `references/calculate-metadata.md`. For media assets, use `/remotion-media`. For Lambda deployment and cost optimization, read `references/lambda.md`.
