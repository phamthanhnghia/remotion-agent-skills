# Animation Rules Index

Atomic motion rules for Remotion. Each rule is a single-concern animation pattern composable with `spring()` and `interpolate()`. Pick 2–4 rules and combine them; do not load blueprints for single-purpose animations.

## Index by tag

### Entrance rules

| Rule ID | Plain language | Primary primitive |
| --- | --- | --- |
| `fade-up` | Element fades in while rising from below | `spring` + `interpolate` opacity |
| `scale-in` | Element scales from 0 to 1 with natural deceleration | `spring` scale |
| `slide-left` | Element slides in from the right | `interpolate` translateX |
| `slide-up` | Element slides in from below | `interpolate` translateY |
| `pop-in` | Element enters with a scale overshoot then settles | `spring` with low damping |
| `reveal-clip` | Text or element reveals behind a moving clip-path | `interpolate` clipPath |
| `typewriter` | Text appears character by character | `interpolate` mapped to char count |
| `word-reveal` | Words enter sequentially with spring stagger | stagger `spring` per word |
| `char-cascade` | Characters enter with stagger, each springy | stagger `spring` per char |

### Exit rules

| Rule ID | Plain language | Primary primitive |
| --- | --- | --- |
| `fade-out` | Element fades out | `interpolate` opacity |
| `scale-out` | Element scales down and fades | `interpolate` scale + opacity |
| `slide-exit-left` | Element slides out to the left | `interpolate` translateX |

### Emphasis rules

| Rule ID | Plain language | Primary primitive |
| --- | --- | --- |
| `pulse` | Element pulses in scale to draw attention | `interpolate` multi-stop scale |
| `shake` | Element shakes horizontally (error state) | `interpolate` multi-stop translateX |
| `glow` | Element glows with an animated box-shadow | `interpolate` boxShadow spread |
| `highlight-bar` | Color bar wipes behind text | `interpolate` scaleX |
| `counter` | Number counts from A to B | `interpolate` Math.round |
| `progress-bar` | Width grows from 0 to target | `interpolate` scaleX or width |

### Background rules

| Rule ID | Plain language | Primary primitive |
| --- | --- | --- |
| `gradient-shift` | Background gradient color shifts over time | `interpolate` r/g/b per channel |
| `particle-drift` | Dots drift slowly across the scene | `random(seed)` positions + `interpolate` |
| `noise-vignette` | Dark vignette pulses subtly | `interpolate` opacity |

## Loading a rule

```
Read rules/<id>.md for the full TSX recipe.
```

Rules are kept short (< 40 lines of TSX each). Load only what the composition needs.
