# 11 · Analytic Cubism (Pablo Picasso and Georges Braque, c. 1910–1912)

**Status:** Built ★★
**Layer:** Material (works on any subject)
**Web fit:** Medium — editorial, galleries, architecture, coffee and wine brands, music, portfolio sites; striking for heroes and section backdrops. Weak behind dense reading: overlapping translucent planes behind text lower legibility, so content needs calm planes of its own.

## Recognisable traits

- Space made of overlapping translucent planes, each with its own light-to-dark gradient, interpenetrating so you can't tell what's in front.
- Forms broken into large faceted triangles whose edges spill past the outline; contours drawn as straight segments that overshoot their corners.
- Multiple viewpoints at once: a profile with a frontal eye, a window view repeated and shifted in several panes.
- Muted palette: browns, ochres, greys and grey-blues; colour is subordinate to structure.
- Collage and stencil elements: stencilled letters (CAFÉ, JOU), newspaper fragments, faux wood grain, dotted and hatched texture patches.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (paper) | `#d6c8ac` | Source. |
| Surface (light plane) | `#ece3cf` | Source off-white; text panels. |
| Text | `#3a2c1e` | Source dark brown. |
| Muted text | `#6e5538` | Source brown (large text only). |
| Ochre | `#b8975f` | Source; accent planes, buttons. |
| Grey / dark grey | `#a8a395` / `#5a5852` | Source neutrals. |
| Grey-blue | `#8e9eab` | Source; links, focus, secondary planes. |
| Window greys | `#c9d3da #8fa3b3 #e4e6e2 #6f8597 #b4c1cb #5f7385` | Source window palette. |

Relationship: monochrome-ish earth and grey planes, each with a light and a dark end; the only "colour" is a cool grey-blue against warm ochre. Large deep-brown areas give it weight (the source build was too light; push more dark planes). Dark reading: deep brown `#3a2c1e` ground, light planes in ochre and grey, off-white text (≈ 10.6:1).

## Typography

Stencil and newspaper lettering are part of the style. Display: **Alfa Slab One** (source, 104px for the stencil word) with stencil breaks cut into it; newspaper headings in **Libre Baskerville 700** (source). Stack: `"Alfa Slab One", "Rockwell", serif` for display, `"Libre Baskerville", Georgia, serif` for editorial headings. Body: a plain serif or sans at 17px on a calm plane. Letters can be rotated (±0.11rad) and offset (±13px) individually in display words only.

## Surfaces, texture and edges

- **Translucent planes:** many SVG polygons, each filled with a linear gradient (start at one vertex, end at a random other vertex) with three stops: 25% mixed with off-white → base colour → 40% mixed with dark brown; opacity 0.18–0.7. Source: 18 large hand-placed planes, 9 fan planes radiating from a point (alternating light/dark, α0.3), and ~70 random 3–4-sided planes. Render once into an SVG or image.
- **Texture patches:** dotted planes (9px diamond grid, r1.6), hatched planes (7px diagonal lines), a lined-paper strip.
- **Braque's dashes:** ~9,000 tiny marks (3–8 × 1.5px), 55% off-white at α0.28 and 45% dark brown at α0.16, giving a brick-like grain. Bake into the background.
- **Passage lines:** ~46 thin straight lines (1.3px, α0.55) crossing everything, tying planes together.
- **Faux wood grain:** wavy lines along the long side with Gaussian bumps around knots (`14·exp(−(u − k)²/0.01)`) and knots as 4 concentric ellipses.

## Shapes and layout

Straight edges only; no radius. Panels are polygons (`clip-path: polygon(...)`) with corners not quite square; outlines are straight segments that overshoot by 10–16px. No drop shadows; depth comes from translucent overlaps. Layout is a collage: a central cluster of overlapping planes, content panels slightly tilted (±1–2°) and overlapping each other, fragments of type tucked at the edges. Keep a clear reading order despite the overlaps.

## Components

- **Buttons:** an ochre or grey-blue polygon with a gradient from light to dark corner and dark brown label; hover shifts the gradient direction.
- **Cards:** off-white polygon panel, one overshooting brown edge line, a translucent plane overlapping one corner.
- **Navigation:** a strip of newspaper-style labels; active item stencilled in Alfa Slab One.
- **Inputs:** off-white panel with a straight brown underline that overshoots both ends.
- **Chat bubbles:** faceted polygons; user in ochre, replies off-white.
- **Tables:** off-white with thin brown rules; header like a newspaper masthead.
- **Badges:** small trapezoids with a two-tone split.
- **Icons:** faceted, straight-edged forms; a two-faced profile or frontal eye for an avatar.
- **Loading:** stencil letters hopping and swapping order.
- **Empty state:** a still life (a glass, a guitar fragment) built from a few planes with one line of copy.

## Motion

- **Stencil word shuffle:** letters stepping at 12fps, cycling orders (`CAFÉ / ACFÉ / CAÉF / CFAÉ`), each step offsetting each letter by up to ±13px and rotating ±0.11rad (hashed per step). Great loader or hover.
- **Shifted window panes:** one picture shown through several panes, each with a different offset `dx = sin(9t + 1.7i)·18`, `dy = cos(7t + i)·8` plus a ±3px jitter at 10fps; translucent triangles slide across each pane at ~260px/s.
- **Newspaper tremble:** a clipping shivering at 8fps (rotation ±0.0125rad, x ±1.5px) with a corner flapping.
- **Facet boil:** facet vertices jitter ±8% of the cell at 8fps and 16–35% of facets change brightness ±12%.
- **Overlap planes:** a few translucent planes over the content slide `sin(6t)·8` px, embedding things in the space.
- **Reduced motion:** static collage; letters in correct order.

## Techniques

- **Facetiser for images, avatars and mascots:** draw the image flat into an offscreen canvas → read pixels → build a jittered triangle grid (random diagonal per cell) → for each triangle, colour = sample at centroid × (0.8 + 0.38·rand), filled with a light-to-dark gradient from vertex 0 to vertex 2 → draw triangles back **unclipped** (skip centroids with α < 120). Not clipping is the key: edges become jagged straight lines and planes spill outside the outline.
- **Cell size:** ~58px for large forms; 26px for detail (hair, small subjects). At 34px everything looks like a low-poly 3D model.
- **Re-add structure after faceting:** two flat planes for a face, a frontal almond eye on a profile, straight-segment contours that overshoot.
- **Colour-filtered second pass:** when re-faceting a sub-area (hair), only process pixels matching its colour range, or the face gets the hair's colour.
- **Stencil breaks:** cut thin horizontal and vertical gaps out of the display letters (`destination-out` on canvas, or an SVG mask), so the planes below show through.

## Performance and accessibility

- Cheap: a baked background, SVG polygons, CSS transforms on a few planes and letters. Expensive: running the facetiser per frame (pixel readback); facet images once and boil by swapping 2–3 pre-baked variants.
- Contrast: dark brown `#3a2c1e` on paper `#d6c8ac` ≈ 8.2:1, on off-white ≈ 10.6:1. Muted brown `#6e5538` on paper is ≈ 4.2:1; large text only. Translucent planes over text drop contrast unpredictably: keep text panels opaque and above the planes.
- Rotated and shuffled letters must not be the only label; provide the readable word to assistive tech.

## Pitfalls

- First frame differs from later ones → offscreen canvas state (line joins/caps) leaks between frames and GPU/CPU readback differs; create contexts with `willReadFrequently: true` and call `ctx.reset()` each frame instead of `clearRect`.
- Facets too small → low-poly 3D look; use big cells.
- Circular clip for a re-facet pass → spills colour into the face; filter by colour.
- Arms the same colour as the body → they melt into the facets; make sleeves darker than the dress.
- Too light overall → loses the gravity of analytic Cubism; add more deep-brown planes.

## Nearest neighbours

- **36 · Picasso blue period:** same painter, but monochrome blue, elongated figures and broad brushstrokes, no faceting.
- **22 · Russian constructivism:** also geometric and typographic, but bold red/black diagonals, halftone photos and propaganda energy, not muted translucent planes.
- **12 · Bauhaus:** geometry with flat opaque primaries and clean edges, not translucent brown facets.

## Source

Adapted from huashu-art-motion · `11_cubism.md` (built ★★; noted shortcoming: too light, not enough large dark-brown planes; figure contours stayed realistic curves under the facets).
