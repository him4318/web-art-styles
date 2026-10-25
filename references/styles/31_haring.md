# 31 · Radiant line figures (Keith Haring, New York street and pop art, 1980s)

**Status:** Built ★★★
**Layer:** Period specimen (strongest as tribute, campaign page or celebration moment), though the thick-line-plus-radiating-ticks vocabulary transfers well to icons and illustrations.
**Web fit:** Medium–High — event and campaign pages, music, kids/education, community and social-cause products, celebratory moments in any app. Low for sober enterprise UI. Draw your own figures in the approach; don't trace or reproduce Haring's actual artworks or iconic motifs one-to-one.
**Mockup:** `assets/mockups/31_haring.png`

## Recognisable traits

- One thick, even, round-capped black line for everything; no line-weight variation, no hatching.
- Flat saturated fills (yellow, green, red, blue, pink) on a single flat ground.
- Featureless outlined figures, mid-dance, merged into one silhouette.
- Radiating motion ticks: short straight dashes around anything that moves or matters (the "energy" marks).
- Everything bounces to a beat: a public, joyful, graffiti-like rhythm.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background | `#ffd51c` | Source: yellow wall. |
| Surface | `#ffffff` (suggested) | White panels with black outline. |
| Ground band | `#16a54a` | Source: green floor. |
| Text / line | `#000000` / `#111111` | The line is the style. |
| Muted text | `#333333` (suggested) | |
| Accent | `#e8262a` (suggested red) | Hearts, primary action. |
| Accent 2 | `#1f5fd6` (suggested blue) | Second figure colour. |
| Pink | `#ff6fae` (suggested) | Third figure colour. |

Relationship: one flat ground colour, figures in one or two other primaries, everything bound by black. No gradients, no tints, no shading. Dark reading (suggested): black ground with the lines turned white or day-glo (the subway chalk drawings were white chalk on black paper), fills stay saturated.

## Typography

Hand-drawn, chunky and upright: **Permanent Marker** for headings, or a heavy rounded sans such as **Rubik** 800 / **Archivo Black** (`"Permanent Marker", "Archivo Black", system-ui, sans-serif`). Body in a plain sans (**Inter**, **Work Sans**). Uppercase, slightly tracked labels feel like protest-sign lettering. Keep the marker face to headings and short labels.

## Surfaces, texture and edges

- No texture at all: flat ground, flat fills, black line. That emptiness is part of the look.
- **Line weight:** the source used 13px at 1080p for everything — on UI, a single fixed weight such as 3–4px for components and 6–8px for illustrations; never mix weights within one drawing.
- **Merged outlines:** to make overlapping parts read as one figure, stroke every part at 2× line width in black first, then fill every part. The overlaps fill over the inner lines and only the outer contour survives. SVG: draw a group of shapes with `stroke-width: 2·LW` underneath, then the same group with fill and no stroke on top.
- **Round caps and joins** (`stroke-linecap: round; stroke-linejoin: round`).

## Shapes and layout

Thick black outlines on every container, square or gently rounded corners (8–12px). No drop shadows, no blur. A ground line (14px black in the source) can anchor a page section, with figures standing on it. Composition is all-over and frieze-like: repeated figures in a row, motion ticks filling gaps. Generous, but not empty — the ground colour carries a lot.

## Components

- **Buttons:** flat red or white fill, thick black outline; on hover, radiating ticks appear around the button.
- **Cards:** white panels with thick black outlines on the yellow ground.
- **Navigation:** outlined pills; active item filled with a primary and surrounded by ticks.
- **Inputs:** white, thick black outline; focus adds ticks at the corners.
- **Chat bubbles:** outlined bubbles in white (replies) and pink or blue (user).
- **Tables:** black rules at line weight; header row filled green.
- **Badges:** small outlined hearts, stars or figures with two or three ticks.
- **Icons:** draw the icon set in the same single-weight round-capped line; add ticks for "active".
- **Loading:** a small figure cycling through dance poses (hard cuts).
- **Done moment:** a heart that pulses with radiating ticks, or several figures jumping.
- **Empty state:** one dancing figure on the ground line.
- **Mascot:** featureless or near-featureless (the source kept a single eye dot and a red lip only so a character was recognisable); same single line weight, no thin detail lines.

## Motion

- **Dance in hard cuts:** 4 poses switched at 8fps with no interpolation — that is the Haring way of moving. CSS: `animation: pose .5s steps(1) infinite` cycling between pre-drawn pose groups.
- **Radiating ticks:** n short dashes on an ellipse around the object (length 22–34, width 7–9 at source scale), re-angled at 8fps.
- **Everything on the beat:** `bounce = |sin(t·BPM/60·π)|^0.6` at 128 BPM. Elements hop (source: 12px), alternate elements are half a beat out of phase; furniture-like objects squash and stretch from their base (`scaleY 1 − 0.07(1 − b)`, `scaleX 1 + 0.05(1 − b)`).
- Hearts pulse, frames rock ±0.025rad, "jump" dashes under figures lengthen on the beat.
- `prefers-reduced-motion`: one static pose per figure, ticks shown still, no bouncing.

## Techniques

```css
.tick-burst { position: relative; }
.tick-burst::after {               /* radiating ticks as a static SVG, swapped by steps */
  content: ""; position: absolute; inset: -14px; pointer-events: none;
  background: url(ticks-a.svg) center/contain no-repeat;
  animation: tick 0.25s steps(1) infinite;
}
@keyframes tick { 50% { background-image: url(ticks-b.svg) } }
.bounce { animation: beat calc(60s / 128) cubic-bezier(.3,0,.7,1) infinite alternate; transform-origin: 50% 100%; }
@keyframes beat { to { transform: translateY(-6px) } }
```

- Limbs as capsules (7 segments plus a round head); stroke all at 2× then fill all, so joints merge.
- Hold a pose library as SVG `<symbol>`s and toggle visibility; don't tween between poses.

## Performance and accessibility

- Very cheap: flat SVG, stepped CSS animations, no filters (the source rendered in ~4ms per frame).
- Contrast: black on `#ffd51c` is ~15:1; black on `#16a54a` is ~6.5:1 (passes), but long body copy still reads better on white panels. White text on red or green fails at small sizes — use black text.
- Motion: a whole page bouncing to a beat is fatiguing and can trigger vestibular discomfort. Confine the beat to one celebratory area or moment, and stop it entirely under reduced motion.

## Pitfalls

- Reversed end caps on limbs left holes at joints under thick strokes → use round caps and the stroke-2×-then-fill order.
- Thin detail lines (hair strands) break the "equal line" rule → delete them; one weight only.
- Window bars or frames drawn over a figure → draw structure lines underneath figures.
- Faces with full features → stops reading as Haring; keep features minimal or none.
- Smooth tweening between dance poses → looks like generic motion graphics; use hard cuts.

## Nearest neighbours

- **13 · Lichtenstein pop:** also pop, black outline and primaries, but pop uses Ben-Day dots, comic-panel realism and variable ink weight; Haring has no dots, one even line, featureless figures and radiating ticks.
- **33 · Rubber-hose cartoon:** also bouncy figures on a beat with thick lines, but rubber-hose is monochrome 1930s film with pie-cut eyes, white gloves and film grain; Haring is saturated flat colour, featureless figures, no film artefacts.

## Source

Adapted from huashu-art-motion · `31_haring.md` (built ★★★; noted shortcoming: the main human figure was not Haring-like enough — keep figures simpler and more silhouette-led).
