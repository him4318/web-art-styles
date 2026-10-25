# 13 · Pop art halftone (Roy Lichtenstein comic panels + Andy Warhol grids, 1960s)

**Status:** Built ★★★
**Layer:** Material (works on any subject)
**Web fit:** High for marketing pages, launches, onboarding, empty states and playful consumer apps; Medium for product UI (use it on chrome and moments, keep data areas calm). Low for long reading or dense dashboards: dots and thick outlines tire the eye.
**Mockup:** `assets/mockups/13_pop.png`

## Recognisable traits

- Ben-Day dots: a regular 45° grid of round dots standing in for tone (skin, sky, shadow), clearly visible at reading distance.
- Thick, even black outlines around every shape (source: 5–6px at 1920 wide), with flat primary fills inside.
- Comic-panel framing: heavy black panel borders, caption boxes ("MEANWHILE…"), speech and thought bubbles, jagged onomatopoeia bursts.
- Shading by black diagonal hatching or a solid black "misregistration" block, highlights as short white strokes; never a smooth gradient.
- Warhol variant: the same image repeated in a grid, each cell recoloured with a different screen-print palette.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (newsprint) | `#f3eedb` | Source "paper". |
| Surface | `#fbf8ee` | Source white; cards and bubbles. |
| Text / outline | `#141414` | Source black; every outline uses it. |
| Accent (blue) | `#2f5cc8` | Wall dot ground; links, primary panels. |
| Accent 2 (yellow) | `#f7d52c` | Caption boxes, primary buttons. |
| Red | `#d8232a` / `#a8161c` | Bursts, lips, alerts; dark red for pressed states. |
| Dot skin | `#fae0cc` ground + `#e2483c` dots | Use for "tone" areas such as avatars or hero fills. |
| Warhol cell grounds | `#9ccf3a` `#e0359a` `#2fc0b0` `#f08a2a` | Rotate through these for a grid of repeated items. |

Relationship: three printer primaries plus black and paper; secondary colours only in Warhol grids. No gradients, no greys except black at reduced opacity for dot overlays. Dark reading (suggested): a black page with newsprint-cream panels, keeping the same primaries; the panel, not the page, carries the text.

## Typography

Display: **Bangers** (the source uses Bangers 400 at 46px for captions and 82px for bursts) with `"Bangers", "Impact", system-ui, sans-serif`. Use it uppercase, tracking +0.02em, often with a black stroke (`-webkit-text-stroke` or a stacked `text-shadow`) and a hard 4px offset shadow. Body: a plain, sturdy sans (suggested **Inter** or **Work Sans** 400–500) in black on cream; comic lettering faces for body text are hard to read. Captions can use a hand-lettered comic face (suggested **Comic Neue** bold) at one size only.

## Surfaces, texture and edges

- **Ben-Day dots with pure CSS:** a two-layer radial-gradient on a square tile gives the 45° grid (four corners plus centre in the source).
  ```css
  .dots { --p: 24px; --r: 5.6px; --dot: #fbf8ee; background-color: #2f5cc8;
    background-image:
      radial-gradient(circle, var(--dot) var(--r), transparent calc(var(--r) + .5px)),
      radial-gradient(circle, var(--dot) var(--r), transparent calc(var(--r) + .5px));
    background-size: var(--p) var(--p);
    background-position: 0 0, calc(var(--p) / 2) calc(var(--p) / 2); }
  ```
  Source sizes at 1920px: wall pitch 24 / radius 5.6 (diagonal spacing ~17px, matched to the original); skin pitch 13 / r 3.3; faint print dots on yellow pitch 14 / r 2.2 at α 0.18. Scale pitch with the layout (suggested: `clamp(14px, 1.25vw, 24px)`).
- **Dots stay fixed to the page:** the source aligns dots to screen coordinates so they don't "swim" with moving shapes. On the web, use `background-attachment: fixed` on large decorative areas, or keep dot layers on elements that don't animate.
- **Panels:** a 5–6px solid black border, square corners, a cream or dot fill. A caption box is a yellow rectangle with a 5px black border pinned to a panel corner.
- **Paper grain (optional):** a faint SVG noise tile at low opacity on the newsprint background.

## Shapes and layout

Square corners everywhere; the outline is the edge. Shadows are hard offsets in black (4–8px, no blur), or none. Lay out pages as comic panels: a CSS grid with thick black gutters (`gap` filled by a black background) and panels of unequal size; one panel per message or feature. Diagonal action lines and bursts can break the grid at one point per screen, not more.

## Components

- **Buttons:** yellow fill, 3–4px black border, 4px black offset shadow, Bangers label; on press the shadow collapses and the button moves 4px down-right.
- **Cards:** cream panel with thick border; an optional caption box in the corner carries the title.
- **Navigation:** a strip of panels; the active item gets a blue dot fill with a cream label slip on top.
- **Inputs:** cream field, 3px black border; focus adds a yellow caption-box tab above it, plus a visible black/yellow outline.
- **Chat bubbles:** real speech balloons (white, black outline, a tail); assistant "thinking" as a thought bubble made of circles.
- **Tables:** keep them plain cream with 2px black rules; no dots behind numbers.
- **Badges:** small starbursts or caption tags in red/yellow.
- **Loading:** the thought-bubble three dots (see Motion).
- **Empty state:** one panel with a caption ("MEANWHILE…") and a single burst.
- **Illustrations and mascots:** outline every part with the same thick black line, fill flat, use dot fills for skin or tone. The source's lesson for hair: a flat wavy shape reads as cloth; recognisable comic hair comes from clustered round curls, each with a black C-shaped stroke inside. Draw all curls' black outlines first, then all fills, so the union has one clean outline.

## Motion

- **Warhol colour cycling:** a grid of repeated items swaps palettes; cell `i` uses palette `(i + step) % 4` with `step = floor(t · 11)`, i.e. a hard swap roughly every 90ms in the source. For UI, slow it down (suggested: every 600–1000ms) and use it on a hero or "celebration" grid only.
- **Thinking dots:** three bubble dots light in turn on a 12-frame cycle (at 60fps): dot k is "on" when `((f − 2k) mod 12) < 4`, growing from r12 to r20 and jumping 6px up. In CSS: three `@keyframes` with staggered delays, `steps(1)`.
- **Burst jitter:** onomatopoeia boxes shake at 30fps with hashed offsets of ±8/±6px and ±0.03rad; surrounding black action lines pulse in length `0.85 + 0.25·sin(40t + 2i)`. Use it for a "done!"/"new!" moment for under a second.
- **Hover:** a panel lifts by growing its offset shadow (4px → 8px), stepped not eased.
- Easing is snappy: `steps()` or very short ease-out (≤120ms). Under `prefers-reduced-motion`, stop colour cycling and jitter, show the bursts static and keep the thinking dots as a static "…".

## Techniques

- **One outline function:** every shape is stroke-then-fill with the same black width. In SVG, set `paint-order: stroke` with `stroke-width` doubled, so the outline sits outside the fill.
- **Hatching for shadows:** inside a clipped region, black diagonal lines 3.5px wide at 10px spacing: `repeating-linear-gradient(45deg, #141414 0 3.5px, transparent 3.5px 10px)` masked to the shadow shape.
- **Union outlines:** for clusters (thought bubbles, curls), stroke all circles first at 11px, then fill all of them white on top. Cheapest way to get one outline around the union.
- **Bursts:** a 14-point star polygon with randomised radii (source: 128/178) as an SVG `polygon` or `clip-path`, outer white, inner yellow, red Bangers text with a 12px black stroke and 4px black drop.
- Dot fills cost almost nothing as CSS gradients or SVG `<pattern>`; never sample pixels to make dots.

## Performance and accessibility

- Cheap: gradient dots, borders, offset shadows. Expensive: huge dot layers repainting during scroll; prefer static or `fixed` layers.
- Contrast: black on newsprint 15.8:1, black on yellow 12.7:1, cream on blue 5.7:1, cream on red 4.7:1 (AA for body only at regular size and above). Never put body text on a dot field: dots behind letters destroy legibility. Put text on a solid panel or caption slip.
- Black on magenta `#e0359a` is 4.5:1, borderline; use those Warhol colours for blocks, not text grounds.
- Colour cycling and jitter can be uncomfortable; keep them short, small and off under reduced motion.

## Pitfalls

- Hair or soft forms painted as one flat wavy shape → reads as a blanket; build them from outlined clusters with interior C-strokes.
- A posed figure's head left at its default position floats above a crouched body → when you re-pose an illustration, move the parts to touch.
- Dots that move with an animated element "swim" → anchor dot patterns to the page.
- Dots behind body copy → unreadable; solid panels for text.
- Everything bursting at once → one burst per view is a punchline; ten is noise.

## Nearest neighbours

- **27 · Kirby (Marvel Silver Age):** also comic halftone and black, but uses four-colour process dots, misregistration and crackling energy dots with dynamic perspective. Pop is calmer, flatter and uses big single-colour dot fields.
- **22 · Constructivism:** also uses halftone, but as variable-size dots on photo greyscale, with red/black diagonals and no outlines. Pop uses fixed-size dots as flat tone with thick outlines.

## Source

Adapted from huashu-art-motion · `13_pop.md` (built ★★★, no recorded shortcoming; the source notes the classic crying-girl tear and a dialogue box were left out and could be added for a more "comic" read).
