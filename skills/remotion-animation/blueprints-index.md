# Animation Blueprints Index

Blueprints are multi-phase scene templates. Each entry is a complete recipe for a common video scene pattern. Load a blueprint only when you need scene-level orchestration — for simple entrances and exits, compose atomic rules from the spring/interpolate adapters instead.

## Available blueprints

| ID | Name | Description | Phases |
| --- | --- | --- | --- |
| `brand-reveal` | Brand Reveal | Logo enters, tagline reveals, holds, clean exit | 4: enter · hold · expand · exit |
| `stat-hero` | Stat Hero | Big number counts up with context label and supporting chart | 4: enter · count · proof · hold |
| `social-proof` | Social Proof | Testimonial card slides in, avatar enters, quote types on | 3: card · avatar · quote |
| `product-callout` | Product Callout | Screenshot enters, feature labels animate in sequentially | 3: screen · labels · CTA |
| `data-story` | Data Story | Chart animates in, key bars/points highlight, insight label appears | 4: axes · bars · highlight · label |
| `timeline-reveal` | Timeline Reveal | Horizontal timeline draws right-to-left, events appear at nodes | 3: draw · nodes · labels |
| `title-card` | Title Card | Full-screen title with animated background and subtitle | 3: bg · title · subtitle |
| `lower-third` | Lower Third | Name + title bar slides in from left, holds, slides out | 3: enter · hold · exit |
| `quote-card` | Quote Card | Quote text types on character by character, attribution appears | 3: frame · quote · attribution |
| `outro-cta` | Outro CTA | Subscribe/follow prompt with animated icon and URL reveal | 3: icon · text · cta |

## Loading a blueprint

```
Read blueprints/<id>.md for the full recipe.
```

Do not read all blueprints speculatively. Load only the one matching the scene you're building.

## When to use a blueprint vs. atomic rules

- **Blueprint:** scene has 3+ distinct choreography phases; exact phase ordering and overlap timing matter; you want runnable starter code for the full pattern
- **Atomic rules:** 1–2 animation properties; entrance + hold + exit is the whole scene; or you're combining parts of multiple blueprints

## Composition patterns that use blueprints

- `/remotion-motion-graphics` → `title-card`, `lower-third`, `brand-reveal`
- `/remotion-explainer` → `stat-hero`, `data-story`, `social-proof`
- `/remotion-product-launch` → `product-callout`, `social-proof`, `outro-cta`
- `/remotion-data-viz` → `stat-hero`, `data-story`, `timeline-reveal`
