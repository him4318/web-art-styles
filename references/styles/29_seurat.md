# 29 · Pointillism (Georges Seurat, *A Sunday on La Grande Jatte*, 1884–86)

**Status:** Built ★★
**Layer:** Material (works on any subject)
**Web fit:** Medium–High — hero illustrations, backgrounds, section dividers, data art and calm editorial pages; the dot renderer can turn any image or illustration into pointillism. Low for small UI chrome: dots below ~6px pitch turn to mush, and text never goes on dots.
**Mockup:** `assets/mockups/29_seurat.png`

## Recognisable traits

- Images built from equal-sized dots of pure colour laid side by side; the eye mixes them (optical mixing) instead of the brush.
- Complementary dots sprinkled through every area: orange in blue shadow, red-violet in green grass.
- Halation: a lighter seam on the lit side of a boundary and a darker seam on the shadow side, exaggerating edges.
- Stillness and order: stiff, frieze-like forms, calm horizontals, a sunny afternoon held still.
- Seurat painted dotted borders around some canvases, which themselves contain complementary dots.

## Palette

The source works from a 21-colour pure palette and never mixes on the "brush":

| Role | Hex | Note |
|---|---|---|
| Background (light) | `#f8f4e8` | Source: the palette's white; use as page ground. |
| Surface (card) | `#fffdf6` (suggested) | Solid panels for text. |
| Text | `#1a2058` | Source: darkest blue — Seurat avoids black. |
| Muted text | `#2a3a9a` | Source blue. |
| Accent (orange) | `#f08a2a` | Source; complementary to the blues. |
| Accent 2 (green) | `#2f9a5a` | Source; grass. |
| Saturated set | `#1a2058 #2a3a9a #3f6fd0 #6aaee6 #1f6a52 #2f9a5a #a8c83a #f2cf3a #f08a2a #e0432a #c0306a #7a4aa8 #f8f4e8 #e8b48a` | Source: 14 pure colours. |
| Tints | `#f6e49a #f6c48a #cfe08a #b0cff0 #f2b8c8 #c99a4a #9a7ac8` | Source: 7 "mixed with white" colours — essential for light areas. |

Relationship: every area is two neighbouring palette colours plus a few complementary dots; light areas use the tints, never white plus scattered saturated dots. Dark reading (suggested): night-time river scene — ground `#141a3a`, dots weighted to the blues and violets, warm lamp dots in orange and yellow; cards remain solid cream.

## Typography

Late-19th-century poster calm: **Cormorant** or **Playfair Display** (regular) for headings, a neutral humanist sans such as **Source Sans 3** for body. Optional: **Dotted** faces only for one display word (e.g. a dot-matrix heading) — never for body. Labels in small caps with +0.06em tracking.

## Surfaces, texture and edges

- **Dot field on a hexagonal grid:** pitch 10px, row spacing 8.66, radius 4.5, each dot with a static ±1.2px jitter (source). At web scale, pitch 8–12px for backgrounds.
- **Underpainting:** beneath the dots, a blurred, slightly desaturated, slightly brightened copy of the flat image (`blur(4px) saturate(.85) brightness(1.06)`), showing through the gaps so the field doesn't look like dots on white.
- **Dotted border:** a ~28px deep-blue band around a hero or illustration, also run through the dot renderer so complementary orange dots appear in it.
- **Panels:** solid cream with a thin dotted rule (CSS `border: 2px dotted` or a radial-gradient row of dots), so content rests on calm areas.

## Shapes and layout

Formal and ordered: clean rectangles, small radii (0–6px), no drop shadows (dots supply all texture). Composition is frieze-like — strong horizontals, profiles, evenly spaced elements. A dotted frame around images is a signature move. Keep plenty of solid areas for text; dots are for illustration and atmosphere.

## Components

- **Buttons:** solid orange or blue with cream text; on hover, the fill breaks into a fine dot pattern of the same colour plus its complement.
- **Cards:** solid cream with a dotted top rule; image cards get the dotted painted border.
- **Navigation:** solid band; the active item marked by a row of dots underneath.
- **Inputs:** cream field with a dotted underline; focus turns the dots orange.
- **Chat bubbles:** solid tints (`#b0cff0` for user, cream for replies) with a dotted edge.
- **Tables:** solid panel, dotted rules.
- **Badges:** single large dots in palette colours.
- **Loading:** a row of dots where colours swap to their pair, like light trembling.
- **Done moment:** the item's flat colour "crystallises" into dots from one corner outward.
- **Empty state:** a small pointillist illustration on cream.
- **Mascot/illustration:** draw flat, then run it through the same dot renderer as the background so it belongs to the picture.

## Motion

- **Light trembling between dots:** a share of dots swap to their paired colour on a stepped clock. The source's first version (4fps, 3% of dots) was judged "basically a freeze frame"; 6fps and 7% gave visible life.
- **Water sparkle:** dots in water areas re-roll their hash at 6fps.
- **Slow drift** of small elements (boats at ~40px/s, a butterfly on `sin(2.6t)·120` with wing flap `|sin(20t)|`).
- The overall tempo is still and serene; motion is shimmer, not movement.
- `prefers-reduced-motion`: no dot swapping; a static dot field.

## Techniques

**Optical-mixing colour choice (the renderer):**
1. Render the target image flat (canvas or an offscreen image).
2. For each dot, read the target colour and add a ±20 hash jitter (R + dj, G + 0.8·dj, B − 0.6·dj) so boundaries between colour pairs interleave instead of forming bands.
3. For every palette pair (i, j), project the target onto the segment Pj→Pi to get a mix ratio w, clamped to [0.2, 0.8]. Score = distance from target + `0.004·|Pi − Pj|²` (penalises distant pairs, so black + white never fakes grey). Pick the best pair; the dot uses Pi if `hash(i, j) < w`, else Pj.
4. Cache results in a Map keyed by 15-bit quantised colour.
5. Replace 8% of dots with the complementary colour (blue↔orange, green↔red-violet, yellow↔violet…; 21 mappings in the source).
6. Halation: compare luminance with the point 14px down-right; if the difference > 45, with 45% probability place a white dot on the bright side and a deep-blue dot on the dark side.
7. Batch dots into one `Path2D` per colour (21 paths) and fill each once — ~24,000 dots in one pass.

For the web, run this once at load in a canvas (or a Web Worker / offline build step) and display the result as an image; animate only a sparse overlay of swapping dots. A cheap CSS approximation for small areas: two offset `radial-gradient` dot tiles in paired colours plus a sparse complementary tile.

## Performance and accessibility

- Expensive: re-running the renderer per frame (~100ms per frame in the source). Cheap: render once, cache, re-render only on resize (debounced), animate ≤7% of dots on a stepped 6fps timer, pause when hidden, cap DPR at 2.
- Contrast: `#1a2058` on `#f8f4e8` is ~14:1. Text must sit on solid panels; dot fields have uneven local contrast and fail for body copy.
- Motion: shimmer is subtle but constant; remove under reduced motion.

## Pitfalls

- Favouring single pure colours (via the penalty) cut smooth wall gradients into vertical colour bands → keep pair mixing and add the target jitter.
- Continuous ratios still switched whole columns at boundaries → banding again; the hash jitter on the target colour fixes it.
- No tints in the palette → light walls broke into white, skin and stray blue dots, like snow noise. Include the "mixed with white" tints.
- Too little motion (4fps / 3%) → reads as a still. Use ~6fps / 7%.
- Grid too regular → mechanical; keep the per-dot jitter.

## Nearest neighbours

- **21 · Kusama dots:** also dots, but Kusama's are large, hand-drawn, irregular and the dot is the subject; Seurat's are small, equal, many-coloured and dissolve into an image.
- **13 · Lichtenstein pop:** also dots, but Ben-Day dots are one mechanical colour on white in a print screen with black outlines; Seurat mixes many pure colours with complements and has no outlines.

## Source

Adapted from huashu-art-motion · `29_seurat.md` (built ★★; noted shortcoming: rather static, grid too regular).
