# 30 · Matisse cut-outs (Henri Matisse, *Jazz*, 1947)

**Status:** Built ★★★
**Layer:** Material (works on any subject)
**Web fit:** High — product UIs, dashboards and marketing pages that want warmth and a handmade feel without losing structure. Weak for dense data tables, where jagged edges add noise.
**Mockup:** `assets/mockups/30_matisse.png` (shown with 16 · 2026 flat grain)

## Recognisable traits

- Shapes cut with scissors: straight short segments with tiny irregular jogs, never a perfect curve or a perfect rectangle.
- Flat, saturated gouache-painted paper: no gradients inside a shape; depth comes from paper overlapping paper.
- Pinned to a wall: each piece casts a soft, slightly offset shadow.
- Organic "seaweed" leaves (a tapered stem with alternating round-tipped fingers), stars and hearts as recurring motifs.
- Strong complementary pairs side by side (cobalt with yellow, red with emerald) on calm grounds.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall) | `#1d4fb8` / `#4f95dc` | Source: *Polynesia* double-blue checkerboard. For UI, use one as the ground or as large blocks. |
| Surface (paper) | `#f7f2e7` (suggested) | Cream paper for cards so text stays readable. |
| Text | `#231b1b` (suggested) | Near-black paper. |
| Accent | `#f2c12e` (suggested yellow) | Stars, primary buttons. |
| Accent 2 | `#d23a2a` (suggested red) | Hearts, alerts. |
| Leaf green | `#1f9a5a` | Source floor colour; success states. |

Relationship: pure flat colours placed edge to edge; a shape never fades into another. Light reading: cream paper on sand or linen. Dark reading: charcoal paper on deep umber, with the same saturated accents.

## Typography

Paper-cut needs a confident, slightly heavy sans for headings: **Geist 800** or **Bricolage Grotesque** (`"Bricolage Grotesque", "Geist", system-ui, sans-serif`). Body stays a neutral sans at normal weight. Labels can be uppercase with +0.04em tracking. Avoid serifs: Matisse lettering in *Jazz* is hand-brushed, so if you need flavour, use a brush-script face for one word only.

## Surfaces, texture and edges

- **Scissor edges:** generate a polygon around each card: sample points every 14–24px along each side and jitter each by ±2–2.5px with a seeded random generator, then apply it as `clip-path: polygon(...)`. Put the clip on a `::before` "paper" layer rather than the element itself, so focus rings and tooltips aren't clipped.
- **Pinned shadow:** `clip-path` removes `box-shadow`, so wrap the paper in an element with `filter: drop-shadow(5px 6px 4px rgba(10, 20, 40, .3))`, or paint a second, offset `::after` sheet in a darker tone.
- **Gouache streaks (optional):** a cached texture of about 2,600 long faint strokes (60–300px long, 6–22px wide, half black and half white, alpha .04–.13, tilted −0.25rad), laid over the page with `mix-blend-mode: overlay`. Render it once to a tile; never per frame.
- **Grain (from 16 · 2026):** a small SVG `feTurbulence` tile thresholded to dots, masked with a gradient so it thickens toward one corner of each sheet.

## Shapes and layout

No border-radius; edges come from the cut polygon. Shapes overlap slightly and sit at tiny rotations (±0.4–0.8°) like pinned paper. Leave generous calm ground around groups, as Matisse did. Grids are fine, but stagger one element per row so it doesn't read as a template.

## Components

- **Buttons:** a cut-paper tab in yellow or cobalt with dark text; on press, the shadow shrinks (the paper is pushed against the wall).
- **Cards:** a cream sheet with a pinned shadow and corner grain.
- **Navigation/sidebar:** one large coloured cut-out (cobalt or plum) as the panel; the active item is a smaller yellow cut-out on top.
- **Inputs:** a cream sheet; on focus, a coloured sheet slides in behind it.
- **Chat bubbles:** the user's bubble is a tilted coloured cut-out; replies are cream sheets.
- **Tables:** dashed rules in ink at 18% opacity; keep cells on a single calm sheet (don't cut each cell).
- **Badges:** small paper tags; stars and hearts as status icons.
- **Loading:** three paper tabs flipping in turn (`scaleX` 1 → 0.1 → 1).
- **Empty state:** a single seaweed leaf on calm ground with one line of copy.

## Motion

- Seaweed sways around its base: `rotate(sin(2.8t + φ) · 0.13rad)`, with the edge "breathing" via `scaleX` 0.93–1.0.
- Stars twinkle by rotating and scaling (`0.82 + 0.22·sin(7t + 1.9k)`); hearts pulse (`1 + 0.18·max(0, sin 9t)`).
- Paper falling or flipping: `scaleX = cos(6t)` reads as a sheet turning over.
- Done moment: a check sticker flips onto the item (rotateY 90° → 0 with an overshoot ease), plus a few paper bits flicked away.
- Easing is soft and springy (paper has weight but no inertia). Under `prefers-reduced-motion`, show everything static and skip the flips.

## Techniques

```js
// Seeded scissor-cut polygon for clip-path (per element)
function cutPolygon(seed, jitter = 2.2) {
  let s = seed; const r = () => ((s = (s * 16807) % 2147483647) / 2147483647);
  const pts = [], side = (n, f) => { for (let k = 0; k < n; k++) pts.push(f(k / n)); };
  const j = () => `${((r() - .5) * 2 * jitter).toFixed(1)}px`;
  side(9, t => `calc(${t * 100}% + ${j()}) calc(0% + ${(r() * jitter).toFixed(1)}px)`);
  side(5, t => `calc(100% - ${(r() * jitter).toFixed(1)}px) calc(${t * 100}% + ${j()})`);
  side(9, t => `calc(${100 - t * 100}% + ${j()}) calc(100% - ${(r() * jitter).toFixed(1)}px)`);
  side(5, t => `calc(0% + ${(r() * jitter).toFixed(1)}px) calc(${100 - t * 100}% + ${j()})`);
  return `polygon(${pts.join(",")})`;
}
```

- Seaweed leaf: one tapered stem plus 2×lobes alternating round-tipped "fingers" (angle `±(1.05 − 0.55u)`, length `w·(1 − 0.45u)`, width `w·(0.34 − 0.12u)`), all added to one path and filled once with `nonzero`, so you get a single union with one shadow. Don't modulate width along a spine: it comes out as a caterpillar or a zigzag.
- Shapes you can't cut point by point (illustrations, mascots): displace the whole layer with a static fbm displacement map (`fbm(0.028x, 0.028y)·9`) using an SVG `feDisplacementMap`, applied once.

## Performance and accessibility

- Cheap: clip-paths, a cached grain tile, and CSS-transform sway. Expensive: re-running the displacement filter on large areas, and animating `filter`. Keep text on cream or charcoal paper, never directly on cobalt with thin weights.
- Contrast: dark ink on cream is ~14:1. White on `#1d4fb8` passes AA; yellow on cobalt doesn't for small text.
- Motion: sway amplitude stays small (≤ 0.13rad); turn it off under reduced motion.

## Pitfalls

- Falling paper crossing a face or text → route decorative motion through empty areas only.
- Dark text on cobalt is unreadable → put a white or cream paper slip under any text on blue.
- Width modulated along a spine for leaves → caterpillars; use lobed fingers in one path.
- Clip-path on the interactive element hides focus rings → clip a pseudo-element instead.

## Nearest neighbours

- **16 · 2026 flat illustration:** also flat colour, but uses no outlines and no cut edges, and builds volume from grain density. Matisse has scissor edges and pinned shadows and builds no volume at all.
- **12 · Bauhaus:** also flat primaries, but geometric and ruler-straight. Matisse is organic and hand-cut.

## Source

Adapted from huashu-art-motion · `30_matisse.md` (built ★★★; noted shortcoming: the gouache streaks were a little heavy).
