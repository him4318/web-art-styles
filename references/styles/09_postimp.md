# 09 · Post-Impressionism (Vincent van Gogh, 1888–1889)

**Status:** Built ★★★ (the source catalogue's benchmark implementation)
**Layer:** Material (works on any subject)
**Web fit:** Medium — memorable marketing heroes, art and culture sites, music, night/sleep products, illustration-led onboarding. Too energetic as the backdrop for long working sessions; the strokes compete with content unless panels are calm and opaque.

## Recognisable traits

- Long, thick, directional strokes that follow a flow field: vertical on walls, converging along perspective on floors, spiralling in skies.
- Many hues in every region (a dozen blues in one wall), not one colour with noise.
- Dark blue contour lines around objects, slightly wobbly, redrawn as if alive.
- Swirls: star halos and lamp halos drawn as rings of strokes; whole skies turning.
- Intense complementary contrasts: cobalt and ultramarine against chrome yellow and orange.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall swatch) | `#7f9be6 #5f7fd8 #a7bbf2 #6c6fd2 #8ad0e6 #c3cbf6 #4f6cc8 #9a8fe0` | Source wall swatch; use as a painted field, not behind text. |
| Night sky swatch | `#22339a #2f4fb8 #5c86dc #8fb6ee #1b2a78 #3c6bd0` | Source; dark reading ground. |
| Floor swatch | `#c0573c #a8453a #d7774a #8e3b30 #5f8f6a #c96a52 #e08a5c` | Source; warm sections. |
| Surface (suggested) | `#f6efe0` | Cream "canvas" panel for text. |
| Text | `#1b2a78` | Deep blue from the sky swatch; also the contour colour. |
| Accent (chrome yellow, suggested) | `#f2c14e` | Stars, lamps, primary buttons (dark text). |
| Accent 2 (orange) | `#e08a5c` | From the floor swatch. |

Relationship: blue fields against yellow/orange lights; deep-blue contours tie everything together. The night reading (deep blue sky, yellow stars, cream panels) is the most iconic; a day reading uses the lighter blue wall with orange floor tones.

## Typography

Headings: an expressive, slightly irregular face: **Fraunces 700** (soft, wonky serif) or a brushy hand like **Caveat Brush** for short display words (`"Fraunces", "Georgia", serif`). Body: a sturdy humanist sans at 17px in deep blue on cream. Labels in small caps with +0.05em tracking. Van Gogh's own letters were cursive; use a script for one signature-style word at most.

## Surfaces, texture and edges

- **Painted fields:** build on a canvas once: flat base painting → stroke pass per region with its own swatch and flow direction → deep-blue contour pass. Ship as an image or draw at load and cache.
- **Three passes, not one** (source parameters): everything except wall/floor: cell 10, length 22, width 6.5; wall: cell 14, length 64, width 8; floor: cell 12, length 46, width 8. Stroke outlines at 0.8–0.9 with 40–45% deep blue mixed in.
- **Contours:** deep blue lines (~4.5px at 1080p) around objects; give them slight width variation and wobble.
- **Panels for UI:** cream canvas panels with a thick, uneven deep-blue border (an SVG path with a few control points jittered) or a border made of short strokes.

## Shapes and layout

Organic, slightly wobbly outlines; radii soft but uneven. Borders are thick (3–5px) deep blue. Shadows are not needed; contours separate things. Composition is dynamic: a big swirling hero (sky, field) with content in calm cream panels that "rest" on it. Place swirls behind empty space, not behind text.

## Components

- **Buttons:** chrome yellow with deep-blue text and a wobbly deep-blue outline; hover rotates a faint stroke halo around it.
- **Cards:** cream panel, deep-blue hand contour, a few short strokes in a corner.
- **Navigation:** a deep-blue night bar with yellow star-like active marker.
- **Inputs:** cream with a deep-blue contour; focus adds a yellow halo ring.
- **Chat bubbles:** cream for replies, yellow for the user, both with blue contours.
- **Tables:** cream with thin blue rules; headers on a light blue stroke band.
- **Badges:** small yellow discs with a ring of strokes.
- **Icons:** filled shapes with deep-blue contours; stars as dots with concentric stroke rings.
- **Loading:** a star with its halo of strokes turning, or a small swirl.
- **Empty state:** a single cypress or a lamp with a halo.

## Motion

- **Swirling sky:** sky strokes follow the tangent of the nearest vortex, rotating slowly (+0.3t). On the web, render the swirl as a static image and rotate a masked copy very slowly, or animate a few stroke-ring overlays.
- **Lamp and star halos:** dashed rings expanding outward and fading, stars twinkling.
- **Flame-like flicker:** strokes in a direction field wavering like flames.
- **Boil:** the source re-seeded strokes and contours at 8fps for a living, hand-redrawn feel. For UI, apply the boil only to contours and small decorations (step between 2–3 pre-rendered variants), never to the large painted field behind text.
- **Done moment:** a yellow burst of radiating strokes around the item.
- **Reduced motion:** static painting, no boil, halos fixed.

## Techniques

- **Flow field:** wall `−π/2 ± 0.22·noise`; floor strokes aligned toward the vanishing point; sky along the tangent of the nearest swirl; halos along circles; vertical for headboards, near-horizontal for sheets. Any surface can get a field: decide one direction per region plus small noise.
- **Region swatches:** sample stroke colour from the base painting, then pull it toward a swatch of 6–9 hues (mix base 0.3–0.45). Colour jitter alone gives "noisy single blue"; the feeling comes from many hues.
- **Props layer:** small objects (furniture, vases, frames, icons) get their own fine stroke pass clipped inside them (`source-atop`) so big strokes don't shatter them.
- **Making an illustration or mascot fit:** flat fill → fine strokes clipped inside → contours and features. The source's "swirl cat" adds 7 rings of orange spiral strokes clipped to the body, turning at 0.6rad/s: a strong signature for a mascot.

## Performance and accessibility

- Expensive: three full-screen stroke passes cost ~110ms per frame in the source, the heaviest in the catalogue. Never per frame on the web: render once (or pre-render 2–3 boil variants) and animate overlays with transform/opacity. Cap DPR at 2; pause when hidden.
- Contrast: deep blue `#1b2a78` on cream `#f2e8d0` ≈ 10.4:1. Cream text on mid blue `#5f7fd8` ≈ 3.5:1 fails for body; use cream on the night blues (`#22339a`, ≈ 9:1) or put text on panels. Dark text on chrome yellow passes easily.
- Swirling, boiling textures are among the most motion-sensitive treatments; keep them still behind reading areas and fully off under reduced motion.

## Pitfalls

- One stroke setting across the whole page → it turns into a carpet; separate passes per region.
- Small objects painted with the big strokes → shattered; give them a props layer.
- Only colour jitter → noisy single hue; use multi-hue swatches.
- Boiling the whole page → nauseating and expensive; boil only accents.
- Text over strokes → illegible; cream panels.

## Nearest neighbours

- **08 · Impressionism:** also visible strokes, but short, light, unoutlined dabs in pastels; no contours, no swirls.
- **19 · Munch expressionism:** also long flowing strokes and a warped world, but the whole image waves in sinuous bands with blood-red skies. Van Gogh keeps objects solid with blue contours, and the swirls stay local.

## Source

Adapted from huashu-art-motion · `09_postimp.md` (built ★★★, the catalogue's benchmark; noted shortcoming: heaviest render at ~120ms per frame). The source card is short; most of the web translation here is suggested.
