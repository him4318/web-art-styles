# 01 · Cave painting (Upper Palaeolithic, c. 40,000–12,000 BCE)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute or easter egg)
**Web fit:** Medium — a striking themed skin for a chat app, game, kids' or history product, or a one-off campaign page. Wrong for dense dashboards and anything that must feel neutral and corporate; the rock texture and charcoal edges add noise around numbers.
**Mockup:** `assets/mockups/01_cave.png`

## Recognisable traits

- Pigment soaked into rock: colour sits *in* a lumpy, grainy limestone surface and gets dirtied by its relief, never on top of it like a sticker.
- Charcoal outlines that are solid at the core with a sprayed, speckled halo and small holes where the pigment didn't take.
- A tiny earth palette: red and yellow ochre, charcoal black, chalk white on warm stone. Nothing blue, nothing green, nothing saturated beyond ochre.
- Firelight: warm, low, flickering light that wanders across the wall, with a dark vignette all round.
- Motifs: running animals (bison, horse) drawn in two alternating poses, sprayed negative hand stencils, embers drifting up.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall, dark/firelit) | `#1d120b` → `#2b190e` | From the app implementation; the source wall is lit ochre `#ce925a` / shadow `#6e4426` / red patch `#b6643a`. |
| Background (wall, light/limestone) | `#e6d8bd` | App implementation's daylight reading. |
| Surface (chalk panel) | `#efdfc0` | Source chalk white `#f0dfbf`; cards and bubbles. |
| Text on chalk | `#24170e` | Charcoal; source charcoal `#21150d`. |
| Text on dark wall | `#f3e6cc` | Chalk, with a soft text-shadow. |
| Accent (red ochre) | `#9c3520` / dark `#74261a` | Headings on chalk, send button, hand prints (`#8e2a1a`). |
| Accent 2 (yellow ochre) | `#cf8f2c` | Active nav, user bubble (`#e2b26a`), highlights. |
| Embers | `#ffe082` | Only in tiny glowing dots. |

Relationship: three earths and charcoal on stone; colours are opaque and flat, then "dirtied" by the wall texture. Light reading = charcoal on sunlit limestone; dark reading = chalk and ochre on a firelit wall at night. Never add blue or a cool grey.

## Typography

Headings and labels in a hand-drawn marker face: **Kalam 700** (`"Kalam", "Geist", ui-sans-serif, system-ui, sans-serif`), letter-spacing 0 for headings and +0.04–0.08em for small uppercase labels. The source title was Kalam 700 at 78px in red ochre, painted *into* the pigment layer so it takes the rock texture too. Body text stays a plain sans at normal weight; Kalam at paragraph length tires the eye.

## Surfaces, texture and edges

- **Rock wall:** a reduced-size SVG `feTurbulence` height field (`baseFrequency ≈ .012 .018`, 5 octaves) lit with `feDiffuseLighting` (light from the upper left), multiplied with a low-frequency ochre stain, rasterised once and stretched full screen. The relief is soft, so it upscales cleanly. Source recipe: `fbm(0.0022x, 4 oct)·1.0 + fbm(0.011x, 3 oct)·0.16 + noise(0.05x)·0.03`, normals ×52, light `(-0.55, -0.62, 0.56)`.
- **Grain:** two tiled noise layers (dark specks and pale specks, ~220px tile) over the wall at 0.55–0.75 opacity.
- **Cracks (optional):** about 10 random-walk lines (turn ±0.45rad per step, step 14–40px, 12% branch chance), each a dark line with a pale line offset 1.6px underneath, so it reads as an incised groove. Bake into the wall image.
- **Painted panels:** cards are chalk `#efdfc0` with a "pits" tile (a few rock-coloured holes where pigment didn't take) and an irregular radius like `16px 22px 12px 26px / 20px 14px 24px 12px`.
- **Charcoal outline:** a `::after` with a 2.4px charcoal border masked by a 96px speckle tile (`mask: url(speckle.svg)`), so the line comes out grainy with holes, plus `box-shadow: 0 0 1.5px` for the sprayed halo.
- **Vignette:** a radial gradient, transparent at the centre to ~`rgba(14,7,3,.82)` at the edges (source: r 260→1180, outer α0.8).

## Shapes and layout

No straight geometric edges: radii are lopsided, lines wobble. Line weight 1.7–2.4px for panels, heavier for animals. Shadows are soft and low (`0 10px 26px -18px rgba(0,0,0,.7)`), never hard offsets. Compositions feel found on a wall: generous dark margins, a few painted panels floating in the light pool, decorative animals and hand prints in the gutters. Keep the fire's light pool where the main content is.

## Components

- **Buttons:** a blob of yellow ochre with an inset charcoal ring and lopsided radius; send button in red ochre with a near-circle `46% 54% 50% 50%` radius.
- **Cards:** chalk panel, pits texture, speckled charcoal outline.
- **Navigation/sidebar:** a darker, smoke-stained stretch of the same wall; hand-lettered items; the current page is painted on in yellow ochre with a charcoal outline.
- **Inputs:** chalk panel; on focus the outline thickens to 3px and turns red ochre.
- **Chat bubbles:** user in yellow ochre, agent on chalk.
- **Tables:** dashed rules in charcoal at ~35% opacity.
- **Badges / counts:** small counts as tally marks (an SVG mask in the current text colour).
- **Icons:** charcoal line icons through the same speckle mask; a small hand print as a status mark.
- **Loading:** a tiny running beast stepping between two poses (`steps(1)`, ~0.24s per cycle) with a Kalam label.
- **Empty state:** a sprayed negative hand stencil on bare rock with one line of copy.

## Motion

- **Two-pose animals:** legs swap between stretched and gathered sets every 7 frames at 60fps (≈ 0.12s), head nods ±0.07–0.12rad, body bobs −3/+1px; horse and bison run in opposite phase. Use `steps(1)` keyframes between two SVG groups.
- **Charcoal "boil":** the grain jumps, the geometry doesn't. Source shifted the charcoal pattern every 1/9s; in CSS, hop the outline `::after` between three tiny offsets (`translate(0.6px,-0.5px) rotate(0.12deg)` etc.) with `steps(1)` over 0.33s, with neighbours out of phase.
- **Firelight:** a radial gradient layer blended with `overlay`, wandering ±40–60px and flickering in strength (`0.14 + 0.11·noise(9.5t) + 0.04·sin 23t`). Animate only `transform` and `opacity`.
- **Embers:** 20–50 small glowing dots drifting upward from the bottom edges over ~5.5s, twinkling with `|sin(6t + 1.9i)|`, fading in and out.
- **Done moment:** a red-ochre hand print pressed onto the finished item (scale-in with a slight overshoot, ~0.5s).
- **Reduced motion:** freeze the fire at a mid value, hide embers, stop the boil, show the beast in one pose; the hand print simply appears.

## Techniques

```xml
<!-- Pigment soaked into rock: rough edge plus speckled holes (from the app implementation) -->
<filter id="pig" x="-10%" y="-10%" width="120%" height="120%">
  <feTurbulence type="fractalNoise" baseFrequency=".06" numOctaves="3" seed="21" result="n"/>
  <feDisplacementMap in="SourceGraphic" in2="n" scale="6" result="d"/>
  <feTurbulence type="fractalNoise" baseFrequency="1.1" seed="4" result="s"/>
  <feColorMatrix in="s" values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 -2 1.75" result="holes"/>
  <feComposite in="d" in2="holes" operator="in"/>
</filter>
```

- **Outline recipe:** stroke once with a sparse charcoal-dot pattern (width 2·lw + 7) for the sprayed halo, again with solid charcoal containing ~7% random holes (width 2·lw), then fill flat to cover the inner half. Painter's order handles overlaps.
- **Dirtying the paint:** after painting, lay a texture over the pigment layer with `source-atop` (canvas) or `mix-blend-mode` on a clipped overlay: rock shadows push toward black, highlights toward warm white, plus damp/dry blotches. Do it as one composite.
- **Negative hand stencil:** a radial spray of dots in a 320² canvas (σ≈46, 3,200 dots, α0.15–0.6, plus a tighter σ≈28 layer), then `destination-out` (or an SVG mask) with a solid hand shape. Bake to an image.
- **Illustrations and mascots:** reduce outlines to 12–16 points and add strong hand jitter; a single almond eye on a profile and flat ochre silhouette read as "drawn on rock". Smooth vector curves look too modern.

## Performance and accessibility

- Expensive: live SVG filters on many elements, a per-frame lighting pass. Cheap: render the wall and all textures once as images (SVG → data URL), boil with `transform`, move the fire with `transform`/`opacity`, embers on one small canvas capped at DPR 2 and paused when the tab is hidden.
- Contrast: charcoal `#24170e` on chalk `#efdfc0` ≈ 13:1; chalk `#f3e6cc` on the dark wall ≈ 15:1; red ochre `#9c3520` on chalk ≈ 5.4:1 (fine for headings and short labels). Put long text on chalk panels; text straight on the relief needs `text-shadow: 0 1px 2px rgba(10,5,2,.6)`.
- Motion: firelight flicker can bother sensitive users; keep it low amplitude and remove it under reduced motion.

## Pitfalls

- Mid-frequency relief too strong (coefficient 0.35, normals ×60) → the wall reads as camouflage or sand dunes; keep it at ~0.16 / ×52.
- Firelight added with `lighter`/`screen` → ochres wash out to pink-white; use `overlay`.
- Pigment layered with `multiply` → chalk white disappears; paint opaque and dirty it afterwards.
- Hand shape built as one combined path → opposite winding cancels and leaves gaps; fill each part into the mask separately.
- Boiling by jittering paths → shapes deform; shift the texture, not the geometry.
- Bigger fire flicker to make it "more alive" → the whole page shakes; add small local motion instead.

## Nearest neighbours

- **20 · Dunhuang murals:** also pigment on an old wall, but uses mineral greens and blues on earth-red ground with flowing long lines and flaking paint. Cave has no blue or green and blunt, speckled outlines.
- **31 · Keith Haring:** also primitive figures with simple outlines, but Haring's lines are clean, uniform and black on flat colour with radiating motion marks. Cave lines are grainy and soaked into rock.

## Source

Adapted from huashu-art-motion · `01_cave.md` (built ★★★; noted shortcoming: the human figure was too cartoon-smooth, not primitive enough; the rock relief is static, so the fire changes brightness but not shadow direction).
