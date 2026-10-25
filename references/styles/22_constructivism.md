# 22 · Russian Constructivist poster (Alexander Rodchenko, El Lissitzky, 1920s)

**Status:** Built ★★★
**Layer:** Material (narrow use)
**Web fit:** High for campaign and launch pages, manifestos, event posters, editorial headers and bold marketing; Medium for product UI (strong for headers, navigation and empty states, calm it down for forms and data). Mind the political associations of the imagery and Cyrillic slogans.

## Recognisable traits

- Diagonals: bars, wedges and text running at steep angles; nothing sits square except the occasional frame.
- Red, black and aged paper only, in flat cut shapes: red wedges, black circles, black bars.
- Photomontage: halftoned greyscale photos cut out with a white margin, combined with crisp hand-drawn geometry.
- Geometric block lettering, slogans stamped big and set vertically or along diagonals; the text is part of the composition, laid over figures.
- El Lissitzky's red wedge piercing a black circle; radiating radio waves; engineering structures (lattice towers).

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (aged paper) | `#e9dfc4`, darker `#d9c9a4` | Source paper. |
| Surface | `#f4ecd6` | Source off-white; cards and photo margins. |
| Text / black | `#151311` | Source black. |
| Accent (red) | `#c8231c` | Source red. |
| Deep red | `#8f1711` | Pressed states, borders on red. |
| Halftone photo greys | `#5e5e5e` → `#e8e8e8` (source sky) | Rendered as black or red dots. |

Relationship: red and black on paper; nothing else except the greys of photographs. Red is used in large shapes, not as a tint. Dark reading (suggested): a black page with paper panels and the same red; the classic reading is light paper.

## Typography

Geometric, heavy, condensed sans, uppercase: suggested **Oswald** 700, **Bebas Neue** or **Russo One** for display (`"Oswald", "Bebas Neue", Impact, sans-serif`), tight to normal tracking, set at angles (rotate −5° to −30°) or vertically. Body: a plain grotesque (suggested **IBM Plex Sans** or **Work Sans** 400), horizontal and black on paper. The source draws Cyrillic block letters on a 5×7 module grid, each letter built from rectangles and quadrilaterals; that grid is useful for custom logotypes. Only use real Cyrillic if you mean it.

## Surfaces, texture and edges

- **Paper:** warm fill plus a subtle noise tile; cached.
- **Halftone photos:** convert greyscale to dots on a 45° grid, dot diameter `cell·gain·√(1 − L)` (source: cell 7 / gain 0.8 for a landscape, cell 4.6 / gain 0.74 for faces), dots only where the source alpha > 128. Pre-process images server-side or in a canvas once; all dots go into one path filled once (~3,000 dots under 2ms). CSS approximation: `filter: grayscale(1) contrast(…)` plus a dot `mask-image` of `radial-gradient` circles; it gives fixed dots rather than variable ones, so pre-processing is better.
- **Cut-out margin:** photo cut-outs get a paper-coloured border (source: 7px `#fbf7ea` stroke around the silhouette), i.e. a `drop-shadow(0 0 0 …)` stack or an SVG stroke behind the masked image.
- **Shape shadows:** shade with halftone dots placed inside the path, not with gradients.
- **Edges:** crisp, ruler-straight; 3px black lines on cut shapes.

## Shapes and layout

Build pages from rotated rectangles, circles and wedges: a diagonal black bar behind the hero heading, a big black circle behind a portrait, a red ground plane sloping across the bottom (source: from y 905 to 856 across 1920px, topped with a 12px black line). Square corners, no soft shadows. Strong asymmetry, one dominant diagonal per view. Overlap freely: slogans sit on top of figures and photos. Use CSS `transform: rotate()` on blocks and `clip-path: polygon()` for wedges; keep the reading flow of body text horizontal.

## Components

- **Buttons:** red rectangles with paper-coloured uppercase labels, or black; a slight rotation (−2°) on a hero CTA only.
- **Cards:** paper panels with a black top bar and a red square in a corner; images as halftone cut-outs.
- **Navigation:** a vertical black bar carrying stacked labels, or a diagonal strip in the header.
- **Inputs:** paper field with a 3px black underline; focus turns it red and adds an outline.
- **Tables:** black rules, bold condensed headers, numbers in red.
- **Badges:** red circles or black squares with tiny condensed type.
- **Loading:** a red wedge thrusting into a black circle, or radio waves expanding from a point.
- **Empty state:** one big slogan stamped diagonally with a red wedge.
- **Illustrations and portraits:** halftone the face or photo, add a paper margin, then draw sharp features (eyes, mouth) with crisp black lines on top; clothes as flat cut shapes with 3px black lines and dot shading.

## Motion

- **Stamped slogans:** letter k stamps in at `0.045k` s, scaling from 1.5× to 1× in 0.12s; then the line does a wave hop `−10·max(0, sin(11t − 0.8k))` px. Every fourth letter black, the rest red. A strong page-entrance or "done" moment.
- **Wedge thrust:** the wedge tip moves back and forth along its 0.34rad axis by `70·(0.5 − 0.5·cos(2π·1.6t))` px.
- **Dashed ring:** a red dashed circle (dash 46/22) rotating via `stroke-dashoffset: −260t`.
- **Radio waves:** four red arcs, `r = 18 + ((1.4t + k/4) mod 1)·230`, opacity and stroke width fading with phase. Good for "live" or "broadcasting".
- **Small sway:** scarf tails `sin(9t)·6`; steam as three black zig-zags rising.
- Easing: mechanical: linear for rotations, short ease-out for stamps. Under `prefers-reduced-motion`: slogans appear set, waves and wedge static.

## Techniques

- Geometric letters: each glyph is several separate rectangles/polygons on a 5×7 grid, each filled separately (see Pitfalls).
- Lattice tower (Shukhov style): stacked hyperboloid sections, each two families of 18 straight lines with a ±1.1rad phase offset; omit the back lines (`sin < −0.2`). An SVG generated once makes a striking hero line drawing over a halftone photo.
- Layering: cache the whole poster background; animate only wedge, rings, waves and slogan letters as separate DOM/SVG elements.

## Performance and accessibility

- Expensive: runtime halftoning of large photos, many animated letters. Cheap: pre-halftoned images, SVG geometry, transform-only animations.
- Contrast: black on paper 14.0:1; paper on red 4.8:1 (AA for body at normal size, just); paper on deep red 7.8:1; red text on paper 4.3:1 (large text or bold labels only). Rotated text is harder to read: keep paragraphs horizontal and rotate only headings.
- Stamping and hopping text should be short and play once; disable under reduced motion.

## Pitfalls

- A diacritic placed above the letter cell (the short mark of Й) gets covered in vertical stacks and the word misreads → keep each glyph's parts inside its own 7-unit cell.
- Building a letter's crossing strokes as one path → overlapping areas cancel under nonzero fill and the letter misreads (А became В) → fill each part separately.
- Slogan baseline too low → letters run off the frame; position by the letter centre.
- Cheek shadows in the greyscale source at full strength → after halftoning they look like a beard; halve shadow opacity before halftoning.
- Text drawn under figures → posters put slogans over the image; z-order the text on top.

## Nearest neighbours

- **12 · Bauhaus:** also geometric and flat, but red-yellow-blue primaries, balanced compositions and right angles. Constructivism is red and black only, aggressively diagonal, with photomontage and slogans.
- **13 · Pop art halftone:** also halftone dots, but fixed-size Ben-Day dots as tone with thick black outlines and comic framing. Constructivist dots vary in size to render photographs, and shapes have no comic outlines.

## Source

Adapted from huashu-art-motion · `22_constructivism.md` (built ★★★, narrow use; noted shortcoming: lacks Rodchenko's megaphone-shaped cone of text; the source suggests a red-and-black wedge of words radiating from a point).
