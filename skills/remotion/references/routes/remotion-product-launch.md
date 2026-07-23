# Route: remotion-product-launch

- **Input:** A website URL, product description, app store page, or brief about a product or service to market.
- **Output:** A 30–90 s promotional Remotion video showcasing the product, typically with screen captures, animated feature callouts, and a call-to-action.
- **Triggers:** "make a promo for", "showcase my product", "product launch video", "app promo", "website walkthrough video".

## Contract

- Requires: `message` (the core value proposition), `product_name`, `destination`, `aspect`
- Optional: `url` (for site capture), `length`, `cta` (call to action text), `style_preset`

## Interview

1. **What is the product and its core value prop?** (must-have)
2. **Product URL?** (if site capture is needed — optional)
3. **Call to action?** ("Try free", "Get started", etc. — optional; defaults to none)
4. **Target platform?** (16:9 / 9:16 / 1:1)
5. **Length?** (15 s / 30 s / 60 s — recommend 30 s)
6. Run-shape questions: (a) storyboard? (b) automation or companion?
