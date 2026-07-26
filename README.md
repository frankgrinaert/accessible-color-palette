# Accessible Color Palette

Build WCAG-compliant color scales from a handful of brand key colors. Each step is generated to hit a target contrast ratio against a background, using [Adobe Leonardo](https://github.com/adobe/leonardo).

Open the app, inspect the grid, and copy tokens as JSON-friendly snippets for your design system.

## Quick start

```bash
pnpm install
pnpm dev
```

Then open [http://localhost:3000](http://localhost:3000).

```bash
pnpm build && pnpm start
```

Requires Node `>=26.5.0` (see `.nvmrc`).

## How it works

1. You define **key colors** (your brand palette).
2. You group them into **scales** (`primary`, `danger`, …) with a color space and smoothing option.
3. Leonardo generates an 11-step scale (`50`–`950`) so each step matches a **target contrast ratio** against `BACKGROUND`.
4. The UI shows HEX, per-swatch contrast, and the minimum contrast across that row. Click to copy.

Contrast uses WCAG 2 relative luminance (via Leonardo).

## Customize

Everything you edit lives at the top of [`color-palette.tsx`](color-palette.tsx).

### Key colors

```ts
const KEYCOLORS = {
  blue: "#174EA6",
  mediumBlue: "#4285F4",
  lightBlue: "#D2E3FC",
  // ...
} as const satisfies Record<string, CssColor>
```

### Scales

Reference key colors by name:

```ts
const colorConfigs: ColorConfig[] = [
  {
    name: "primary",
    keys: ["blue", "mediumBlue", "lightBlue"],
    colorSpace: "OKLCH",
    smooth: true,
  },
  // danger, warning, success, neutral, ...
]
```

| Field | Meaning |
| --- | --- |
| `name` | Scale id used in tokens (`primary-500`, …) |
| `keys` | Key colors Leonardo interpolates between |
| `colorSpace` | `CAM02`, `CAM02p`, `LCH`, `LAB`, `HSL`, `HSLuv`, `HSV`, `RGB`, `OKLAB`, `OKLCH` |
| `smooth` | Smoother (`true`) or sharper (`false`) transitions |

### Background, steps, and ratios

```ts
const BACKGROUND = "white" as CssColor
const COLOR_STEPS = [50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950]
const CONTRAST_RATIOS = [1.05, 1.13, 1.28, 1.6, 2.2, 3.3, 4.8, 7.5, 11.3, 15, 18]
```

`COLOR_STEPS` and `CONTRAST_RATIOS` are paired 1:1. Change either list carefully so they stay the same length.

## Copying tokens

| Action | UI |
| --- | --- |
| One color | Click a swatch |
| One scale | **Copy JSON** under the column |
| Full palette | **Copy JSON (all)** |

Copied lines look like:

```json
"primary-500": "#4285F4",
```

## Project layout

```
app/                 Next.js App Router shell
color-palette.tsx    Config, Leonardo generation, and UI
```

## Stack

- Next.js 16, React 19, TypeScript
- Tailwind CSS 4
- `@adobe/leonardo-contrast-colors`

## Further reading

- [Designing accessible color systems](https://stripe.com/blog/accessible-color-systems) — Stripe
- [Using color at scale for aesthetics and accessibility](https://www.youtube.com/watch?v=B6Qk_j9UGU8) — Ashley Seto, Config 2023
- [Accessible Palette: stop using HSL for color systems](https://www.wildbit.com/blog/accessible-palette-stop-using-hsl-for-color-systems) — Wildbit
- [Perceptually uniform color spaces](https://programmingdesignsystems.com/color/perceptually-uniform-color-spaces/index.html) — Programming Design Systems
- [Color Spaces](https://ciechanow.ski/color-spaces/) — Bartosz Ciechanowski
- [WCAG 2.1 — relative luminance](https://www.w3.org/TR/WCAG21/#dfn-relative-luminance)
