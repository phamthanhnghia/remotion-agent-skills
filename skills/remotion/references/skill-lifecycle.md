# skill-lifecycle.md

## Install and update

Domain skills are self-contained — each skill directory includes its own setup instructions in SKILL.md. There is no centralized CLI command to install Remotion workflow skills.

### Fresh project setup

```bash
# Scaffold a new project
npx create-video@latest

# Install required packages based on the workflow
npm install remotion react react-dom

# Additional packages based on domain skill needs:
npm install @remotion/google-fonts      # remotion-creative / typography
npm install @remotion/lottie            # remotion-animation / lottie adapter
npm install @remotion/captions          # remotion-media / captions
npm install @remotion/media-utils       # remotion-media / audio-reactive
npm install @remotion/lambda            # remotion-cli / Lambda rendering
npm install @remotion/transitions       # remotion-animation / scene transitions
npm install @remotion/zod-types zod     # remotion-core / InputProps schema
npm install recharts                    # remotion-data-viz / recharts
npm install @react-three/fiber three    # remotion-animation / Three.js
```

### Upgrade workflow

```bash
# Check available upgrades (dry run, read-only)
npx remotion upgrade --dry-run

# Apply upgrades
npx remotion upgrade

# Verify all compositions still load after upgrade
npx remotion studio src/index.ts
```

After upgrade, verify by scrubbing all compositions in Studio. If any composition fails, revert:

```bash
git checkout package.json package-lock.json
npm install
```

### No-CLI fallback

If `npx remotion` is unavailable:

1. Check Node.js version: `node --version` (must be ≥ 18)
2. Install locally: `npm install remotion`
3. Use package.json scripts: `npm run dev`, `npm run build`, `npm run render`
4. For rendering without CLI: use the Node.js API (`renderMedia` from `@remotion/renderer`)

## Project health check

Before each render session:

```bash
# Verify Node.js version
node --version

# Verify Remotion version consistency (all remotion packages must match)
npx remotion --version

# Verify Chrome is available
npx remotion install-executables  # installs local Chrome if not found

# Open Studio and check all compositions
npx remotion studio src/index.ts
```
