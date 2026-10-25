# 28 · Water lilies (Claude Monet, Giverny, 1899 onward)

**Status:** Built ★★
**Layer:** Period specimen (water-lily pond and Japanese bridge as tribute); the trembling-water brushwork transfers as a background material.
**Web fit:** Medium — calm hero backgrounds, wellness, gardening, travel or editorial pages, a quiet "focus" screen. Low behind dense UI: full-screen broken brushwork is visually noisy and expensive to animate.

## Recognisable traits

- The water surface fills the picture: a vertical blue-violet field with no horizon, sky known only from its pale reflections.
- Short, mostly horizontal brushstrokes laid side by side; colours mix in the eye, never outlined.
- Lily pads as foreshortened ellipses with a notch, in clusters, some with pink or white flowers.
- Willow reflections as soft vertical bands that waver.
- Coloured shadow: warm yellow on the lit side, violet in shadow — no grey, no black.
- Optional period motif: the green arched footbridge with hanging wisteria.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (water, top → bottom) | `#b9c7e6` → `#8fa6d6` → `#6c84bf` → `#5a72a8` | Source: vertical water gradient. |
| Surface | `#f6f3ee` at 88% (suggested) | Milky panels floating on the water. |
| Text | `#2b2f4a` (suggested) | Deep blue-violet, never black. |
| Muted text | `#5a4f8a` | Source: the fine-line colour for features; good for secondary text. |
| Accent (lily pink) | `#e88aa0` | Source pink set: `#f0a0b8 #f7d0dc #fff2f4 #e88aa0`. |
| Accent 2 (leaf / bridge green) | `#3f8a62` / `#8fdcae` | Source: bridge base and highlight. |
| Leaf greens | `#5f8f4a #7fae5a #3f6f4a #9cc070 #6f9a7a #b4cc78` | Source leaf palette. |
| Water strokes | `#5b6fb5 #7d8fd0 #9db5e0 #6aa0a8 #b7a6d8 #d9c8e8 #8fb8a8 #a6c4e8 #c8b0d8` | Source water palette (mixed ~45% with the base). |
| Sparkle | `#fffbe8 #fdf0d0 #f8e0ec #e8f4ff` | Source: light glints on water. |

Relationship: cool blue-violets dominate, broken by greens and a little warm pink and cream; warm/cool contrast replaces light/dark contrast. No black anywhere. Light reading: a pale morning pond (top of the gradient). Dark reading (suggested): dusk pond `#1f2846 → #2f3d6b` with the same strokes darker and glints warm.

## Typography

Soft and literary: **Cormorant Garamond** or **EB Garamond** for headings (light/regular, generous size), a humanist sans such as **Source Sans 3** or **Lato** for body (`"Source Sans 3", "Segoe UI", sans-serif`). Avoid heavy weights; let headings feel painted-in by using the deep violet rather than black.

## Surfaces, texture and edges

- **Brushstroke field:** render once to a canvas or image — short strokes on a jittered grid. Source passes: water cell 9, length 34, width 6.5, **horizontal** angle ±0.06rad; background foliage cell 6, length 13, width 4.5 (diagonal in the upper half, horizontal below); wooden floor cell 12, length 34, width 8; frames cell 6. No outlines on any pass.
- **Lily pads:** ellipses (rx 38–78, ry 12–21) with a wedge notch, ~40% with a pink/white flower; draw them into the base before strokes so strokes break them up.
- **Willow bands:** vertical bands whose edges waver `sin(0.03y − 3.2t + k)·14`.
- **Panels:** milky translucent cards with soft edges (a small blur on a `::before` or a few stroke marks along the edge), so they float on the pond instead of cutting holes in it.

## Shapes and layout

Soft, no outlines, no hard shadows. Rounded or brush-edged panels. Composition: an all-over field (no focal horizon), so content can sit anywhere; keep a calm area of plain water where text sits and put the lily clusters and glints toward the edges. Wide horizontal format suits hero banners (think the long Orangerie panels).

## Components

- **Buttons:** soft pink or leaf-green pill without outline; hover adds a small warm glow on the lit side.
- **Cards:** milky floating panels with a blur(18px)-style cream halo around them (the source needed this to make an on-screen counter readable over strokes).
- **Navigation:** text on a plain-water band; active item marked with a lily-pad ellipse underneath.
- **Inputs:** cream field, thin violet underline.
- **Chat bubbles:** cream bubbles; user bubbles in pale lilac `#d9c8e8`.
- **Tables:** plain cream panel, faint violet rules; no strokes behind data.
- **Badges:** small lily-pad ovals.
- **Loading:** a ripple — a thin dashed ellipse (ry = rx·0.22) expanding and fading.
- **Done moment:** a lily flower opens on the item, with one ripple.
- **Empty state:** a single cluster of pads on calm water.
- **Illustration/mascot:** flat fill, then fine strokes clipped to its silhouette (cell 6, length 12, colours sampled from its own tints), then a warm gradient over the lit side (alpha ~0.22) and violet over the shade side (~0.32), then only the essential features in fine violet line.

## Motion

- **Light trembling on water (the motif):** ~160 short bright horizontal strokes whose brightness is `clamp(sin(t·(5..11) + φ)·sin(2.3t + i))`, hopping ±5px sideways at ~10fps. Web: a small canvas or a set of absolutely positioned dashes with staggered opacity keyframes.
- **Lily pads drift:** `x + sin(1.3t + φ)·7 + 6t`.
- **Ripples** expand and fade; willow bands waver; wisteria clusters (9 dots each, decreasing size) swing `sin(2.4t + 0.9i)·8`.
- Keep the stroke field itself static; only glints, pads and ripples move.
- `prefers-reduced-motion`: a still painting — no glints, drift or ripples.

## Techniques

- **Render the stroke field once** (canvas at load, or a pre-made WebP) and use it as a background. Animate a separate sparse overlay for glints.
- **Separate layers for important shapes:** the bridge disappeared into the foliage palette until it got its own layer — a dark shadow backing `#24452a` behind it, a 20px `#3f8a62` base stroke and a `#8fdcae` highlight, with its own small-cell stroke pass. Same rule for logos or key UI over a stroke field: give them a dark or light backing so the strokes don't swallow them.
- **Coloured shadow on any illustration:** two `source-atop` (or SVG masked) linear gradients — warm yellow over the lit side, violet over the shaded side.
- **Wisteria as dotted chains**, not thick lines (lines read as glossy pills).

## Performance and accessibility

- Expensive: re-rendering thousands of strokes per frame (the source took ~105ms per frame and 30% of pixels changed, mostly from water "boil"). Cheap: static field + a few hundred moving glints on a small canvas, paused when the tab is hidden, DPR capped at 2.
- Contrast: deep violet `#2b2f4a` on cream panels is well above AA. Never put body text straight on the stroke field; the mid water `#6c84bf` with white text is only ~3.7:1.
- Motion: flicker of glints should be soft and slow; remove under reduced motion.

## Pitfalls

- Whole-screen stroke "boil" at 6fps → 47% of pixels changing, pure noise. Keep the field still; animate only sparse glints.
- Wisteria in thick strokes → smooth pills. Use dotted chains.
- Text over strokes is unreadable → add a soft cream halo or a solid panel.
- Key shape swallowed by the surrounding palette → give it its own layer and backing.
- Mixing spaces (a wall that is a pond, a floor that is wood) felt odd in the source → on a page, let the water be one coherent field rather than patching it into unrelated regions.

## Nearest neighbours

- **08 · Impressionism (general):** same short broken strokes, but a general landscape/figure recipe with pastel regional palettes; Monet's card is specifically the water field — horizontal strokes, lily pads, reflections, no horizon.
- **29 · Seurat pointillism:** also optical mixing, but with equal round dots on a regular grid and complementary dots; Monet uses directional strokes of varied length and a looser, more atmospheric surface.

## Source

Adapted from huashu-art-motion · `28_monet.md` (built ★★; noted shortcoming: the pond-as-wall / wood-floor space felt awkward).
