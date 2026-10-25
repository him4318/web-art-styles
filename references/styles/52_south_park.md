# 52 · Digital cut-paper with limited acting (South Park-style, 1997–present)

**Status:** Research recipe — one redone single-scene study (original kids at a snowy bus stop, a bus pulls in and they hop aboard), self-scored ~75, independently reviewed ~63. Below 80; techniques are hypotheses.
**Layer:** Material (works on any subject) — the cut-paper construction and limited-animation acting transfer to any original characters and scenes.
**Web fit:** Medium — playful marketing, onboarding, error and empty states, explainer pages, mascots and stickers for casual consumer products. Low for serious or enterprise product UI.

This card describes the general visual approach of the show's digitally rebuilt construction-paper look (the producers have publicly said they reconstruct cut paper digitally). It does **not** describe, copy or recolour any of the show's characters, towns, logos or jokes. Design your own characters and avoid colour combinations that point to named characters.

## Recognisable traits

- Everything is coloured paper cut with scissors: geometric, simple silhouettes with slightly wobbly cut edges and faint paper fibre.
- Characters are a few big shapes: round head, trapezoid body, stubby black legs, round mitten hands; a couple of key features only.
- Eyes are two white ovals touching each other on the face, with black dot pupils that show where the character looks; the mouth is a black half-oval that opens and closes.
- Flat fills with one thin black outline; no gradients, no 3D.
- The camera faces the scene straight on; space is built by layering, not perspective.
- Limited acting: pieces pivot at neck and shoulders and slide as whole blocks; pose switches and pauses carry the meaning.

## Palette

Hexes from the source study's original scene (winter bus stop); suggested UI roles.

| Role | Hex | Note |
|---|---|---|
| Background (sky paper) | `#c9dce6` / `#bcd3e0` | Pale blue paper; page ground. |
| Surface (snow paper) | `#f4f7f8` / `#e6edf0` | Cards and panels. |
| Text / outline | `#14120f` | Near-black paper; ~17.4:1 on snow. |
| Muted (road grey) | `#6c7378` | Secondary text; ~4.5:1 on snow, so 16px+ only. |
| Accent (bus yellow) | `#f0b92a` / light `#f6cc55` | Primary buttons with black text (~10.4:1). |
| Accent 2 (sign red) | `#c8402e` | Alerts, stop/cancel; white text on it ~5:1. |
| Mountains | `#e9eef0` with shade `#a9b9c6` | Large background shapes. |
| Wood / pine | `#6b4a30` and greens (suggested `#3f6b45`) | Scenery. |
| Character fills | `#d65a7a`, `#5a78c8`, `#8a8f3a`, `#d9d2c0`, hats `#4a4a52`, `#d6803a`, `#8a3a5a`, `#2f6a58` | Original combinations from the study. |
| Skin | `#f5cfa8` | Generic paper skin for original characters. |

Relationship: flat, bright, cheap-looking paper colours with a black line; no shading except a flat darker shape where needed. Dark reading (suggested): night sky paper `#26303a`, snow cards stay light, outlines stay black.

## Typography

A chunky, friendly sans for headings: **Fredoka 600** or **Baloo 2 700** — `"Fredoka", "Baloo 2", system-ui, sans-serif`. Body in a plain sans (**Inter**, 16px). Don't imitate the show's logo lettering. Labels can be uppercase, +0.02em.

## Surfaces, texture and edges

- **Cut edges:** sample each shape's outline every 18px (10–12px for small circles and eyes) and jitter points by ±2.4px (±0.8–1.2px for small parts) with a seeded random — the same idea as Matisse's scissor polygon but with a **thin black outline (3–3.6px)**. Because `clip-path` can't be stroked, draw these as SVG `<path>`s with `stroke="#14120f"`, or put the clipped shape over a slightly larger black clipped twin.
- **Paper fibre:** a cached tile of ~9,000 tiny 3–15px strokes per 1920×1080, half warm grey at 16% and half white at 50%, multiplied over everything at low opacity.
- **Layers:** sky band, mountain paper, snow layers, road, props, characters — each a separate flat sheet; no blur, no depth of field.

## Shapes and layout

Shapes are geometric and blocky: trapezoids, rectangles, circles, triangles stacked in three layers for pine trees. Black outline on most pieces; background sheets sometimes without. No shadows, no radius beyond the cut wobble. Layout is frontal and horizontal: a ground line, things standing on it, layered left to right. Keep UI panels as larger paper sheets "pinned" in the scene.

## Components

- **Buttons:** bus-yellow paper with black outline and black text; on press, the whole paper piece drops 2px (no shadow change, since there is no shadow).
- **Cards:** snow-white paper with cut edges and a black outline.
- **Navigation:** a road-grey paper strip with yellow dash marks as separators; active item in yellow paper.
- **Inputs:** white paper with a black outline; focus = a second thick yellow outline outside (3px), easy to see.
- **Chat bubbles:** white paper bubbles with a cut tail; a mascot's mouth opens while it "talks".
- **Badges:** small paper circles and octagons (red octagon for "stop/error" without words).
- **Loading:** a mascot's mouth flapping, or paper snowflakes stepping down.
- **Empty state:** an original paper character shrugging (arm pivots), one line of copy.
- **Mascots:** build from 6–10 parts with pivots (neck, shoulders), 3–4 mouth states, and pupils that move within the eye ovals.

## Motion

- **Stepped time:** decorative motion at 12fps (`steps()` timing) — e.g. paper snow discs falling in visible jumps.
- **Whole-block moves:** a vehicle or panel slides in as one rigid sheet with ease-in-out (the study's bus: 3s slide, wheels do not turn).
- **Snap actions:** a door slides open in 0.3s; a card "pops" in with a 1–2-frame hold, no squash.
- **Pivot acting:** heads tilt and arms swing around fixed pivots; hop = a quick vertical jump of the whole character.
- **Gaze:** pupils move up to ±6px inside the eye ovals to point at the thing the user should look at.
- **Talking:** the mouth half-oval opens 4 → 24px; past ~40% open, show a red inner mouth.
- Under `prefers-reduced-motion`, stop snow and talking; keep characters in a neutral pose and use instant state changes.

## Techniques

```css
/* stepped paper snow */
.flake { animation: fall 4s steps(48) infinite; } /* 48 steps in 4s = 12fps */
@keyframes fall { to { transform: translateY(110vh); } }
/* pivot acting */
.arm { transform-origin: 50% 0; transition: transform .12s steps(2); }
.char[data-pose="wave"] .arm.r { transform: rotate(-120deg); }
```

- Generate each cut path once from a seeded jitter and keep it fixed; animating the jitter itself makes paper "boil", which this style doesn't do.
- Tie the fibre texture to the part, not the screen, so it moves with the part (source rule).
- Walking and boarding need state (foot contact, queue order); the study learned to queue characters in single file so they don't overlap.

## Performance and accessibility

- Cheap: static SVG paths, transform-only animation, stepped timing. Avoid re-generating paths per frame.
- Contrast: black on snow and black on bus yellow pass easily; white on sign red ~5:1; grey `#6c7378` is borderline (~4.8:1 on white, ~4.5:1 on snow `#f4f7f8`) — fine for 16px body, avoid for smaller text.
- Gaze and mouth changes should not be the only carrier of information; pair with text.
- Humour: borrowing the look doesn't mean borrowing offensive jokes (source rule).

## Pitfalls

- Copying or recolouring recognisable characters, hats or colour combos → design original characters; the study replaced a hat colour that came too close to a known one.
- Characters all from one mould (source shortcoming) → vary proportions, clothing and silhouette per character.
- Boarding/entering as a plain slide plus hop (source shortcoming) → add pose changes and a beat of pause.
- Mouths not matched to dialogue (source shortcoming) → at least open on stressed syllables.
- Smooth 3D, gradients or thick paper-craft shadows → the look is flat digital paper.
- Puppets that glide for the whole scene → limited acting still needs pose changes and contacts.

## Nearest neighbours

- **37 · Rick & Morty (built):** thin dark rounded outlines and more flexible, linear character drawing. Cut-paper has wobbly cut edges, blocky geometry and rigid pivots.
- **34 · Shadow puppet (built):** translucent leather lit from behind, rivet joints, rods. Cut-paper is opaque, front-lit and has no rods.
- **30 · Matisse:** also scissor-cut paper, but painted gouache, organic shapes, pinned shadows and no outline; this is cheap flat paper with a black line and characters.

## Source

Adapted from huashu-art-motion · `52_south_park.md` (research recipe; self ~75, independent ~63; known shortcomings: kids from one mould, boarding is only a slide plus hop, no lip-sync; subject still close to the show's opening).
