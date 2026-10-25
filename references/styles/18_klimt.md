# 18 · Klimt golden phase (Gustav Klimt, Vienna Secession, c. 1907)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium for luxury, jewellery, wine, opera and gala pages, premium tiers and celebration screens; Low for everyday product UI, where patterned gold fights with content. Works best as one gold surface framing calm content.
**Mockup:** `assets/mockups/18_klimt.png`

## Recognisable traits

- Gold leaf as the ground: mottled, uneven metal with faint seams where square leaves were laid.
- Dense flat ornament on robes and backgrounds: spirals, concentric circles, eye-like ovals, clusters of dots, and black-and-white rectangles.
- The key contrast: realistic, softly modelled faces and hands against completely flat decorative pattern.
- Flowered meadows and mosaic-like fields of small round flowers; gold dust glinting.
- Pattern is clipped inside silhouettes; the body disappears into ornament and only face and hands stay "real".

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (gold leaf) | `#784e12` → `#c49434` → `#f6d880` | Source gold ramp `[120,78,18] → [196,148,52] → [246,216,128]`. |
| Surface (dark) | `#141010` (suggested) | Black panels with gold text, like the robes' black blocks. |
| Surface (light) | `#f6d880` (light gold) or `#f4ead8` (suggested ivory) | For long text. |
| Text | `#2a1a0c` (suggested) on gold/ivory; `#f6d880` on black | |
| Accent | red / white / blue concentric circles | Source ornament trios: red-white-blue, blue-gold-red, purple-white-gold. Hexes not given in source. |
| Skin ramp | `#fbe8da` → `#f0cfb8` → `#d8a88c` → `#b88068` | Source: radial gradient for faces and hands. |
| Pattern brown | deep brown ground under gold spirals (suggested `#3a2410`) | |

Relationship: gold dominates; black, white and small jewel colours (red, blue, green, purple) sit inside ornaments only. Flesh tones are the only soft gradients. Both readings: gold-on-black for a dark theme, black-on-gold/ivory for light.

## Typography

Vienna Secession lettering: tall, geometric capitals with unusual proportions. Suggested **Poiret One** or **Cinzel Decorative** for display only (`"Poiret One", "Cinzel", Georgia, serif`), uppercase, wide tracking (+0.12em), often set in gold on black or inside a framed block. Body: a refined serif such as **Cormorant Garamond** or **EB Garamond** at 400, on ivory or black, never on mottled gold.

## Surfaces, texture and edges

- **Gold leaf tile (pre-render once):** per pixel `f = 0.5 + fbm(0.004x)·0.55 + noise(0.003x, 0.08y)·0.5 ± 0.05`, mapped onto the gold ramp; a very faint square seam every 96px for individual leaves. Then multiply a small (480×270) upscaled fbm for brown-gold mottling, and scatter ~9,000 gold and dark dust specks. Export as a seamless WebP tile.
- **CSS approximation:** a base `linear-gradient` across the gold ramp, an SVG `feTurbulence` tile multiplied over it, and a faint `repeating-linear-gradient` grid for the seams.
- **Ornament layers:** pattern drawn once into an SVG or tile, then clipped to a silhouette or panel with `mask`/`clip-path`. Clip the whole pattern group at once (see Pitfalls).
- **Black-and-white rectangles:** a strip of tall black and white blocks with gold edges, used as borders and dividers.
- **Outlines:** ornamental robes carry a 3px dark outline; faces only a 1.2px faint line. Keep that contrast for illustrations vs UI.

## Shapes and layout

Panels are tall, framed rectangles (square corners, a thin gold rule, sometimes a black-and-white checker frame). Ornament fills large areas edge to edge; content sits in a calm window inside it. Composition: a strong vertical, decorative columns or a robe-like side panel full of pattern, and one clear area for text. No soft shadows; depth is flat.

## Components

- **Buttons:** black with a gold rule and gold uppercase label; primary as a solid gold-leaf tile with dark text. Hover: the diagonal shine (see Motion) crosses the button.
- **Cards:** ivory panel framed by a thin gold line, an ornament strip along one edge.
- **Navigation / sidebar:** the "robe": a tall panel tiled with spirals, eyes and circles; items on a black label band for legibility.
- **Inputs:** ivory field, gold underline; focus adds a dark outline.
- **Tables:** plain ivory with thin gold rules; ornament only in the header band.
- **Badges:** small concentric-circle medallions in jewel colours.
- **Loading:** an eye ornament blinking, or a spiral rotating slowly.
- **Success:** gold-leaf flakes fluttering down and a shine sweep.
- **Empty state:** a single spiral medallion on gold.
- **Illustrations and portraits:** model faces and hands realistically (radial skin gradient, soft blush and eye-socket shadow, faint 1.2px line) and keep everything else flat ornament. That contrast is the style. Keep pattern off faces.

## Motion

- **Eye ornaments blink in turn:** each closes when `sin(2.2t + φ) > 0.86`. A gentle idle for decorative panels.
- **Spirals rotate:** each at up to ±3rad/s in the source; for UI, much slower (suggested ≤0.5rad/s) and only a few.
- **Diagonal shine:** a 520px-wide band at α .24, sheared `matrix(1, 0, −0.45, 1, 0, 0)`, additive (`mix-blend-mode: plus-lighter` or `screen`), sweeping across at constant speed (source: 1.15s across 2,500px). Use it on hover, on a "premium" badge or as an entrance.
- **Gold dust:** specks rising ~26px/s, flashing into four-point stars when `sin(t·sp + φ) > 0.55`; star radius `s·(0.6 + 1.1a)`.
- **Falling flakes:** square and round gold flakes flipping as they fall (~150px/s) for a celebration.
- **Flowers nodding:** `sin(4.2t + φ)·0.35r`.
- Easing: linear shine, smooth sines elsewhere. Under `prefers-reduced-motion`: no shine sweep, flakes or blinking; keep a static gold surface.

## Techniques

- Ornament recipe (source): ~150 random points split into concentric circles in three colours (30%), gold spirals on deep brown (32%), eye-like ovals (22%), white dot clusters with a red heart (16%); plus 60 tall black-and-white rectangles with gold edges at the hem. Generate once to SVG with a seeded random generator.
- Meadow: ~260 concentric-circle flowers growing larger toward the viewer (6 → 20px).
- Clip a whole pattern group in one pass: in canvas, draw all ornaments to an offscreen layer, then `source-in` the silhouette once; in SVG, one `clipPath` on a `<g>`.

## Performance and accessibility

- Expensive: per-pixel gold noise at runtime, hundreds of animated ornaments, full-screen additive sweeps. Cheap: a pre-rendered gold tile, a static ornament SVG, one CSS-animated shine band (`transform` only).
- Contrast: dark brown on light gold 12.0:1 and on mid gold 6.1:1; light gold on black 13.5:1; mid gold on black 6.9:1. But mottled gold varies across its surface: never put body copy directly on gold leaf; use an ivory or black panel.
- Gold specular shine and twinkling can be busy; limit to short moments and respect reduced motion.

## Pitfalls

- Clipping each ornament separately with `source-in` → each pass erases the previous ones; clip the whole layer once.
- Shine band at α .38 → reads as a lens flare; keep it around .24.
- Stars too big → Christmas decorations; shrink them (radius `s·(0.6 + 1.1a)` instead of `s·(1 + 2.2a)`).
- Spiral ornaments on faces → "looks like a rash"; keep pattern on bodies and panels only, and use fewer.
- Intermittent blinks and soft gradients alone leave long static stretches → add slow ambient drift (rising dust, flakes) if the surface must feel alive.
- Body text on patterned gold → unreadable; calm panel for text.

## Nearest neighbours

- **05 · Gothic illuminated manuscript:** shares the gold-leaf technique (the source reuses it), but with brushed and embossed gold on parchment, vines and drop caps. Klimt uses mottled leaf with spirals and eyes, and realistic faces.
- **10 · Mucha Art Nouveau:** same era and decorative ambition, but variable-width organic outlines, halos and pastel colour. Klimt is flat ornament and metal, with almost no outline around patterned areas.

## Source

Adapted from huashu-art-motion · `18_klimt.md` (built ★★★, period specimen; noted shortcoming: the hand on the table didn't connect to the sleeve).
