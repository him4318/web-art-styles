# 27 · Cosmic comic-book (Jack Kirby and four-colour print, 1960s)

**Status:** Built ★★
**Layer:** Period specimen (strongest as tribute, launch page or easter egg)
**Web fit:** Medium — great for launch pages, game or entertainment products, onboarding with "panels", achievement moments. Low for calm product UI: heavy blacks, sound-effect lettering and energy dots are loud. Describe the approach; never reuse publisher characters, logos or trade dress.

## Recognisable traits

- Four-colour newsprint: flat colours built from halftone dots on yellowish paper, with the colour plates slightly out of register against the black line.
- Heavy black ink: thick outlines plus big "spotted black" shadow shapes on the side away from the light, edged with parallel feathering lines.
- **Kirby krackle:** clusters of black dots of varying size that read as crackling cosmic energy.
- Comic grammar: panels and gutters, close-up insert panels, caption boxes ("MEANWHILE…"), extruded 3D sound-effect lettering in a yellow starburst, speed lines.
- (Target, not yet achieved in the source) blocky squared-off fingers, extreme foreshortening and muscular tension — this is what separates Kirby from Lichtenstein-style pop.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (paper) | `#f4ecd6` | Source: newsprint. |
| Surface | `#fffaf0` (suggested) | Caption boxes, cards. |
| Text / ink | `#111111` (suggested) | Pure-ish black ink. |
| Muted text | `#4a4438` (suggested) | |
| Accent (process red) | `#e0302a` (suggested) | SFX letters, primary buttons. |
| Accent 2 (process yellow) | `#ffd400` (suggested) | Starbursts, caption boxes, highlights. |
| Cyan / blue | `#2aa4e0` / `#1d4f9a` (suggested) | Halftone fields (source: light-blue wall with cyan dots, dark-blue floor with purple dots). |
| Magenta | `#d6247a` (suggested) | Insert-panel backgrounds. |

Relationship: CMYK primaries at full strength, mixed only optically through dots, all held together by black. No gradients except those faked with dot size. Light reading only, really; a "dark" version would be a black-dominant cosmic panel with yellow/orange energy and krackle, still on paper-tinted cards.

## Typography

- Display / SFX: **Bangers** (source used Bangers at 132px scaled 0.82 for "SLURRP!") — fallback `"Bangers", Impact, "Arial Black", sans-serif`.
- Captions and UI labels: **Comic Neue** bold uppercase, or a hand-lettered face such as **Patrick Hand**, at small size with +0.02em tracking. Captions are uppercase in comics; keep paragraphs in sentence case with a neutral sans (**Inter**) for readability.

## Surfaces, texture and edges

- **Halftone fields:** CSS `radial-gradient(circle, <ink> r, transparent r+0.5px)` tiled at pitch p, rotated via a wrapper or `background` on a rotated pseudo-element. Source parameters: wall cyan dots pitch 9, r 2.7, 15°; floor purple dots pitch 9, r 3.2, 75°; skin red dots pitch 7, r 1.5, 75°.
- **Misregistration:** draw the colour layer and the ink layer separately and offset the colour layer by (4px, 3px). On the web: colour fills on one layer, black outlines on a layer above with `mix-blend-mode: multiply` (white = transparent), shifted a few px.
- **Paper:** warm cream ground plus faint grain; optional faint yellowing toward the edges.
- **Ink outlines:** 6px at the source's 1080p scale — roughly 2–3px on UI elements, heavier on illustrations.

## Shapes and layout

- Panel grid with white gutters (8–16px) and thick black panel borders; break one panel's border for emphasis.
- Hard offset shadows in black, no blur (`box-shadow: 4px 4px 0 #111`).
- Square corners, except caption boxes with tiny radii and SFX bursts with spiky polygons (source burst: 26 points).
- Asymmetric, dynamic: one big hero panel, one tall insert close-up, captions pinned to corners.

## Components

- **Buttons:** yellow or red fill, thick black outline, hard black offset shadow; on press, the shadow collapses and the button shifts into it.
- **Cards:** comic panels; a narrow caption box in the top-left for the card's label.
- **Navigation:** a row of small panels or caption tabs; active one in yellow.
- **Inputs:** white field with thick black outline; the label as a caption box.
- **Chat bubbles:** speech balloons — white, black outline, a pointed tail; thought bubbles for "thinking" states.
- **Tables:** a single panel with thin black rules; avoid halftone behind numbers.
- **Badges:** small starbursts with one word ("NEW!").
- **Loading:** krackle clusters pulsing, or a caption "MEANWHILE…".
- **Done moment:** an SFX word pops out (overshoot ease) in a yellow burst with extruded black letters.
- **Empty state:** an insert close-up panel with a caption.
- **Mascot/illustration:** pass it through the same two layers — flat colour with halftone, then black ink with spotted-black crescents — so it matches the page.

## Motion

- **Krackle energy:** a cluster of 16–26 black circles, distance `R·rnd^0.7`, radius larger near the centre `R·(0.05 + 0.2·(1 − d/R))`, reseeded at 12fps, each radius pulsing `sin(18t + i)`. Clusters can travel along an S-shaped energy stream.
- **Speed lines:** black lines trailing a moving element, length `70–180·v`, jittering at 12fps.
- **SFX pop:** `outBack`-style overshoot scale-in, then ±3px jitter at 12fps.
- **Insert panel:** a close-up whose pupils follow the action (e.g. toward the cursor), with radiating focus lines reseeded at 12fps.
- Motion is stepped at 12fps (`steps()` easing) to feel printed, not smooth.
- `prefers-reduced-motion`: show SFX and krackle static at final size; no jitter, no reseeding.

## Techniques

- **Spotted blacks for any shape:** shadow = shape ∩ ¬(shape translated toward the light by d1). In canvas: `clip(path)`, then clip with a full-screen rect plus the translated path using `evenodd`, then fill black. A second offset d2 gives a band that you fill with parallel lines along the light direction (spacing 9px, width 2.6px) = feathering. Source offsets (d1/d2): large shapes 16/34, medium 14/30, small 10/22. In SVG: the shape filled black, masked by the translated shape in black on white.
- **Ink layer occludes itself:** fill each part white then outline black, in back-to-front order, so later parts hide earlier lines — no need to compute visible lines.
- **Extruded SFX:** draw the black word 7 times, each offset by (0.8d, d), then the coloured word on top; clip the top 46px to yellow for a two-colour letter. In CSS: stacked `text-shadow: 1px 1px 0 #111, 2px 2px 0 #111, …`.
- **Rings around a sphere:** split into a back half-arc drawn before the sphere and a front half-arc after.

## Performance and accessibility

- Cheap: halftone as CSS gradients, ink as SVG strokes, stepped CSS animations. Expensive: per-frame canvas krackle across a large area; dozens of `text-shadow` layers on animated text (pre-render the SFX as SVG instead).
- Contrast: black on cream ~17:1. Never place body text on halftone or on red; captions use solid white/yellow boxes with black text (black on `#ffd400` ~13:1).
- Motion: 12fps jitter can be tiring — limit it to one moment at a time and disable under reduced motion.

## Pitfalls

- SFX lettering full-size over the focal point covered the subject's face → place bursts in empty corners and scale them down (0.82).
- A big black ellipse used as shadow made a cat read as tortoiseshell → keep spotted blacks as narrow crescents hugging the edge.
- Planetary rings crossing the planet → split into back and front arcs.
- Ending up with "clean pop comic" (Lichtenstein) instead of Kirby → add squared fingers, forced perspective, krackle, and feathered blacks; Ben-Day dots alone don't make it Kirby.

## Nearest neighbours

- **13 · Lichtenstein pop:** also Ben-Day dots and black outlines, but pop is a single enlarged panel, cool and static, with mechanical even dots; Kirby is multi-panel, kinetic, with spotted blacks, feathering, krackle and misregistered plates.
- **22 · Russian constructivism:** also loud graphic print with bold lettering, but constructivism is red/black geometry and photomontage halftones; Kirby is figurative, cosmic and full four-colour.

## Source

Adapted from huashu-art-motion · `27_kirby.md` (built ★★; noted shortcoming: reads more like Lichtenstein — lacks Kirby's perspective and blocky hands).
