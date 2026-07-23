# calculateMetadata Reference

`calculateMetadata` is an async function attached to a `<Composition>` that runs **once before the render begins**. Use it to:

- Dynamically set `durationInFrames`, `fps`, `width`, or `height` based on props
- Fetch external data before any frame renders
- Pre-process heavy computations (transcription analysis, audio data)
- Validate and transform input props

## Basic usage

```tsx
import {Composition} from 'remotion';

type MyVideoProps = {
  items: string[];
};

const MyVideo: React.FC<MyVideoProps> = ({items}) => {
  // items is guaranteed to be available before frame 0
  return <AbsoluteFill>...</AbsoluteFill>;
};

export const RemotionRoot: React.FC = () => (
  <Composition
    id="MyVideo"
    component={MyVideo}
    fps={30}
    width={1920}
    height={1080}
    durationInFrames={150} // fallback default
    defaultProps={{items: ['A', 'B', 'C']}}
    calculateMetadata={async ({props}) => {
      // duration depends on number of items
      const framesPerItem = 45;
      return {
        durationInFrames: props.items.length * framesPerItem,
      };
    }}
  />
);
```

## Async data fetching

```tsx
calculateMetadata={async ({props}) => {
  // Fetch data that will be passed back to the component
  const response = await fetch(`https://api.example.com/data/${props.id}`);
  const data = await response.json();

  return {
    durationInFrames: data.frames,
    props: {
      ...props,
      fetchedData: data, // merged back into props
    },
  };
}}
```

> **Important:** `calculateMetadata` is the only safe place for `fetch()`, `fs.readFile()`, or any async operation that must complete before frame 0.

## Return shape

```ts
type CalculateMetadataReturn = {
  durationInFrames?: number;  // overrides <Composition> prop
  fps?: number;                // overrides <Composition> prop
  width?: number;              // overrides <Composition> prop
  height?: number;             // overrides <Composition> prop
  props?: Partial<Props>;      // merged with incoming props
};
```

## InputProps schema validation (Zod)

Use `@remotion/zod-types` + `zod` for typed props with Studio input:

```tsx
import {z} from 'zod';
import {zColor} from '@remotion/zod-types';

const mySchema = z.object({
  title: z.string(),
  color: zColor(),
  duration: z.number().min(1).max(300),
});

<Composition
  id="MyVideo"
  component={MyVideo}
  schema={mySchema}
  defaultProps={{title: 'Hello', color: '#ffffff', duration: 60}}
  calculateMetadata={({props}) => ({
    durationInFrames: props.duration * 30,
  })}
/>
```

This enables the Studio props editor to show typed inputs with validation.
