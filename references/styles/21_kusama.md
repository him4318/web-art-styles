# 21 · Infinity polka dots (Yayoi Kusama, 1960s–present)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium for art, fashion and event pages, campaign microsites, celebration and loading screens; Low for product UI at full strength. Reduced to a dot motif on one surface, it can work in a playful brand.

## Recognisable traits

- Dots everywhere: walls, floor, objects and bodies covered in hand-painted, slightly irregular dots that dissolve the boundary between figure and ground ("self-obliteration").
- Strong two-colour pairings: red with white dots, yellow with black dots, white with red dots.
- The yellow pumpkin with rows of black dots, largest on the central ridge, shrinking toward the edges.
- Infinity mirror rooms: points of light repeating in a regular grid into endless depth.
- Dots sized and spaced on a near-regular hexagonal lattice, but each dot a little off in size, shape and position.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (red) | `#d7141a` (suggested) | Source gives no hexes for this card. |
| Dots on red | `#ffffff` | |
| Surface | `#ffffff` (suggested) | Cards, with red dots at the edges. |
| Text | `#111111` (suggested) | On white or yellow. |
| Accent (pumpkin yellow) | `#f5c400` (suggested) | With black dots. |
| Dots on yellow | `#111111` | |
| Mirror-room lights | six bright colours on near-black (suggested) | Source: 6 colours, not specified. |

Relationship: one ground colour and one dot colour per surface, high contrast, no gradients. A dark reading exists in the mirror rooms: a black ground with small bright points. All values here are suggested; do not treat them as the artist's colours.

## Typography

A bold, slightly rounded sans with friendly, round counters, suggested **Poppins** 700 or **Nunito** 800 for headings (`"Poppins", system-ui, sans-serif`), black on yellow or white on red. Body: the same family at 400 on solid white. Keep dots out of letterforms; never put text over a dot field.

## Surfaces, texture and edges

- **Hand-painted dot fields:** two staggered hexagonal lattices (source: big dots 30px at 104px spacing plus small 10px dots offset between them), each dot jittered ±25–50% in size and position, slightly elliptical and randomly rotated. Generate once with a seeded random generator into an SVG pattern tile large enough (e.g. 4×4 periods) that repetition isn't obvious.
- **Wrap the lattice by its real period:** when tiling or scrolling dots, take positions modulo the true hexagonal period (row spacing × 2 rows), or the seam shows.
- **Perspective floor:** rows getting larger and sparser toward the viewer (6 → 28px), squashed to 0.42 vertically: a separate SVG layer under a hero.
- **Dots clipped to shapes:** dots inside an illustration or a card edge are a pattern layer clipped by the shape (`mask` or `clipPath`), with its own colour pair.
- **Edges:** shapes are outlined in a bold line (source: 4.5px for a figure) when figure and ground share the same pattern, so they don't dissolve completely.

## Shapes and layout

Round and organic: circles, pumpkins, soft blobs; no sharp corners. Shadows none. Layout is immersive: one or two large dotted surfaces (a hero, a sidebar) and calm solid panels for content. Use scale contrast: a field of tiny dots next to a few huge ones.

## Components

- **Buttons:** solid yellow pills with black text and a few black dots at one end; or white pills on red.
- **Cards:** white with a dotted border band or one corner dissolving into red dots.
- **Navigation:** a red rail with white dots; the active item is a solid white pill.
- **Inputs:** solid white, bold black outline, focus with a yellow ring.
- **Tables:** plain; dots only in the header bar.
- **Badges / avatars:** circular, dotted in the inverse pair.
- **Loading:** a dot field breathing outward from a point (see Motion), or the pumpkin breathing.
- **Empty state:** a single yellow pumpkin.
- **Celebration:** the screen fills with a red field of white dots that swell from where the user clicked, then settle as a calm lattice.
- **Illustrations:** cover characters in dots with their own colour pair (white dots on a red dress, white dots on orange fur, red dots on white areas), and keep a clear outline between figure and ground.

## Motion

- **Breathing wave:** every dot's radius is multiplied by one travelling wave from an origin: `wave = 0.82 + 0.28·sin(0.012·dist − 6t)`. One function makes the whole surface pulse outward in rings. For UI, use the click point or the active element as the origin; slow it down (suggested `− 2t`).
- **Crawling dots:** dots inside a shape scroll downward (source: 46px/s), offset taken modulo the lattice period.
- **Pumpkin:** breathes `scale(1 ± 0.025·sin(5t))` from its base; dot sizes follow the wave.
- **Mirror room:** a 13×7×3 grid of lights flying toward the viewer (z cycling at 0.3/s, perspective focal 150), each a small solid core with a 3.2× soft glow, additive.
- **Self-obliteration finale:** a red overlay fades in while a 120px hexagonal lattice swells from the origin to 0.47× its spacing, then holds still (breathing gently). Cap the radius.
- Easing: smooth sines. Under `prefers-reduced-motion`: static dots, no wave, no flying lights, no overlay sweep.

## Techniques

- CSS for a regular (non-jittered) field: two offset `radial-gradient` layers as in pop halftone; for the hand-painted look, a pre-generated SVG tile.
- For the breathing wave on a moderate number of dots (hundreds), SVG circles with radius computed per frame, or a canvas redraw; for thousands, a WebGL shader or a fragment of CSS: animate `background-size` of a dot layer masked by a radial gradient travelling outward.
- Pumpkin: five elliptical lobes drawn outside in; per lobe, rows of black dots, largest on the central column (9.5px) and shrinking toward the sides and ends.

## Performance and accessibility

- Expensive: per-frame recomputation of thousands of dots; additive glows. Cheap: static SVG tiles, one transformed mask for the wave, a pre-rendered mirror-room loop.
- Contrast: black on yellow 11.5:1, black on white 18.9:1, white on red `#d7141a` 5.2:1. But a dotted surface has no single background: text must sit on a solid panel.
- Pulsing high-contrast patterns can cause discomfort and, at certain frequencies, trigger seizures; keep dot waves slow (well under 3 cycles per second), avoid full-screen red/white flashes, and stop all motion under reduced motion.

## Pitfalls

- Lights scattered randomly with big glows → mushy bokeh; a regular grid with small solid cores reads as infinite mirrors.
- Dots growing without limit → the screen turns white; cap the radius and end on a lattice, not a flat colour.
- Figure and ground with the same pattern → the figure dissolves; add a heavy outline and different dot pairings.
- Tiling seams → wrap positions by the real hexagonal period.
- No resting state at the end of an animation → hold the final lattice for a moment.

## Nearest neighbours

- **13 · Pop art halftone:** also dots, but small printed Ben-Day dots on a strict grid as tone, with thick black outlines. Kusama's dots are hand-painted, larger, irregular and the subject itself.
- **29 · Seurat pointillism:** also built from dots, but tiny dots of many colours mixing optically into a painted scene. Kusama uses two flat colours per surface and never blends.

## Source

Adapted from huashu-art-motion · `21_kusama.md` (built ★★★, period specimen; noted shortcoming: figure and wall shared the same colours and relied on outlines to stay apart). The source gives no hex values; all colours here are suggested.
