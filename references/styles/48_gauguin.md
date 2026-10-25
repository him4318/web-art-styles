# 48 · Gauguin (Paul Gauguin, Synthetism, late 1880s–1890s)

**Status:** Research recipe — one redone single-scene study (women in white caps before a vermilion field, after Gauguin's Brittany pictures), self-scored ~65 on the card and ~74 after a head redo, no independent review. Below 80; techniques are hypotheses.
**Layer:** Material (works on any subject) — large symbolic colour fields with dark blue contours transfer to any subject. The source is explicit that tropical subjects, skin tones and "exotic" ornament are **not** the style and must not be used as shortcuts.
**Web fit:** Medium-high for brand and marketing pages, posters, campaign landers, editorial covers; bold enough to carry a homepage. Medium for product UI: the flat fields and thick contours work as chrome around calm content panels.

## Recognisable traits

- Large flat fields of non-natural colour (a vermilion ground, a yellow-ochre slope) carry mood and structure; colour is chosen for meaning, not realism.
- Simplified forms held by thick, uneven contours in deep ultramarine or violet-black — never pure black.
- Depth compressed: no perspective vanishing; near and far told by overlap, cropping and a raised ground plane.
- "Flat is not flat": inside each colour field you can see brush streaks running in one direction, slightly lighter and darker.
- Strong diagonals (a tree trunk slicing across the picture) and figures cropped by the frame edge.

## Palette

From the source study's anchors (*Vision after the Sermon*, *The Yellow Christ*, *Where Do We Come From?*) — observed, not official.

| Role | Hex | Note |
|---|---|---|
| Background (colour field) | `#d63a1c` | Vermilion field: hero bands, section backgrounds. |
| Ground 2 | `#e0a526` | Yellow-ochre slope; alternate sections. |
| Surface (white cap) | `#efece2` | Cream-white panels for reading; shade side `#a9b4cf`. |
| Text / contour | `#1d2a52` | Deep ultramarine contour; ~11.8:1 on cream. |
| Dark field | `#1b1a22` | Black dress; footer, dark mode ground. |
| Accent (violet-brown) | `#5b2c48` / `#8d4a66` | Tree trunk diagonal; secondary buttons, dividers. |
| Accent 2 (green) | `#2f5a3a` | Foliage; success, tags. |
| Yellow fruit | `#f2c335` | Small highlights, stars, badges. |
| Skin / warm | `#e8b078` / `#d49a66` | Illustration only. |

Relationship: two or three big saturated fields per view, bound by ultramarine contours; small complementary accents (yellow on green, cream on vermilion). Colour fields stay consistent per object — never randomly recolour (source rule). Light reading: cream panels on vermilion/ochre. Dark reading: cream text on `#1b1a22` with vermilion and ochre fields as accents. Cream text on vermilion is only ~4:1 — use large/bold.

## Typography

Gauguin lettered titles in plain painted capitals. Display: **Young Serif** or **Fraunces 700 (soft)** in uppercase with +0.03em tracking, or a sturdy grotesque like **Bricolage Grotesque 700** — `"Young Serif", "Fraunces", Georgia, serif`. Body: a neutral sans (**Inter**, **Source Sans 3**) at 16–18px on cream. Don't use "tribal" or "tropical" display faces.

## Surfaces, texture and edges

- **Streaked flat fill (cloisonné painting):** fill the shape, then clip a set of streaks inside it: 40–160px long, 8–30px wide, alpha 0.55, all near one direction (± 0.12 rad), each streak the fill colour shifted by ±18–34 in value. On the web, pre-render one greyscale streak tile per direction and lay it inside the shape with `mix-blend-mode: soft-light` (or bake per-colour tiles).
- **Contours:** 5–8px ultramarine `#1d2a52`, round joins, slightly uneven. In SVG, stroke the path twice (6px at 95% + a 2px offset stroke at 40%) for variation; or apply a gentle `feDisplacementMap` (scale 2–3) to the stroke only.
- **Panels:** smooth closed-curve shapes (rounded blobs, not polygons) with the contour; text sits on cream-white panels with almost no internal streaking.

## Shapes and layout

Shapes are smooth and simplified, never zig-zag: the source found polyline figures read as memes, so always use smoothed closed curves. No shadows: depth is overlap. Layout uses a few big colour zones split by a strong diagonal, with elements cropped by the viewport edge (a big shape running off-screen). Content panels overlap the colour zones. Avoid grids of identical cards; vary size and let one element dominate.

## Components

- **Buttons:** vermilion or ochre blob with ultramarine contour and dark text (on ochre) or bold cream text (on vermilion, ≥ 18px bold). Press: contour thickens 1px.
- **Cards:** cream-white panel, ultramarine contour, a small streaked colour field as header band.
- **Navigation/sidebar:** a full-height colour field (violet-brown `#5b2c48` with cream text, ~9.4:1); active item is an ochre blob behind the label.
- **Inputs:** cream field, 2px ultramarine contour; focus = contour thickens to 3px plus a yellow `#f2c335` outer ring.
- **Chat bubbles:** user in ochre with dark text, replies in cream; both outlined.
- **Tables:** cream ground, ultramarine hairlines at 30%; no streaks inside cells.
- **Badges:** small outlined blobs in green, yellow, violet.
- **Empty state:** a single simplified object (a tool, a cup) on a coloured field.

## Motion

- **Swaying foliage blobs (source motif):** a bush shape shifts `sin(1.6t + 1.7i)·7px` horizontally and `sin(2.1t + i)·3px` vertically; its contour moves with it.
- **Figure sway:** whole shapes rock by ±8px (`sin(2.2t + seed)`); motion is of whole flat pieces, not internal deformation.
- **Walk cycle (illustrations):** legs swing `sin(7t + φ)·26px`, body bob `|sin 7t|·4px`.
- **Done moment:** a colour field floods in behind the item (scale 0.9 → 1 with a slight overshoot), contour last.
- Never re-colour fields randomly over time. Under `prefers-reduced-motion`, keep everything still.

## Techniques

```js
// Streaked flat fill on canvas (pre-render to an image for the web)
function flat(ctx, path, fill /*[r,g,b]*/, { angle = 0.7, vari = 18, n = 46, bounds, line = '#1d2a52', lw = 6, rnd }) {
  ctx.save(); ctx.fillStyle = `rgb(${fill})`; ctx.fill(path); ctx.clip(path); ctx.lineCap = 'round';
  const [bx, by, bw, bh] = bounds;
  for (let i = 0; i < n; i++) {
    const x = bx + rnd() * bw, y = by + rnd() * bh, L = 40 + rnd() * 120, a = angle + (rnd() - .5) * .25, d = (rnd() - .5) * 2 * vari;
    ctx.strokeStyle = `rgba(${fill[0] + d},${fill[1] + d * .8},${fill[2] + d * .7},.55)`; ctx.lineWidth = 8 + rnd() * 22;
    ctx.beginPath(); ctx.moveTo(x, y); ctx.lineTo(x + Math.cos(a) * L, y + Math.sin(a) * L); ctx.stroke();
  }
  ctx.restore(); if (lw) { ctx.strokeStyle = line; ctx.lineWidth = lw; ctx.lineJoin = 'round'; ctx.stroke(path); }
}
```

- Streak counts used in the study: full-screen field 520, ground 300, trunk 180, small shapes 4–16. Variation (`vari`) 26–34 on big fields, 6–10 on small ones.
- Illustration lesson: draw hat brims and overlapping pieces behind the face, never covering it; use profile heads with clear silhouettes.

## Performance and accessibility

- Cheap: pre-rendered streak tiles, SVG shapes with strokes, CSS transforms. Expensive: generating streaks live, or displacement filters on large areas. Generate once.
- Contrast: ultramarine on cream ~11.8:1; dark on ochre ~7.9:1; cream on vermilion ~4:1 (large/bold only); cream on violet-brown ~9.4:1. Never set body text on a streaked field.
- Large saturated red can be fatiguing; keep it to bands, not the full reading background.

## Pitfalls

- Uniform black-outlined vector fills → generic flat illustration; add streaks and use ultramarine, uneven contours.
- Polyline (angular) figures → read as memes; use smoothed curves.
- Hat or hair drawn over the face → draw it behind.
- Childish small figures, a too-straight trunk (source shortcomings) → keep figures simple but adult-proportioned; give diagonals a slight curve.
- "Exotic" decoration as the style signal → the colour logic is the style; subjects come from the brief.

## Nearest neighbours

- **16 · 2026 flat illustration (built):** also flat colour, but no outlines and volume from grain density. Gauguin has heavy ultramarine contours, visible streaks, and symbolic, non-natural fields.
- **30 · Matisse:** flat saturated colour too, but cut-paper edges, pinned shadows and no outlines. Gauguin is painted, outlined, and builds compressed space with overlap.
- **49 · Chagall:** Chagall tilts, floats and breaks spatial logic; Gauguin keeps the ground and gravity, just flattens them.

## Source

Adapted from huashu-art-motion · `48_gauguin.md` (research recipe; self ~65 → ~74 after head redo, no independent review; known shortcomings: boy and goat still childish, trunk too straight, missing broad violet-brown shadows and symbolic small animals).
