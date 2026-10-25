# 45 · Renoir (Pierre-Auguste Renoir, Impressionism, 1870s–early 1880s)

**Status:** Research recipe — one single-scene study exists (a garden dance under trees), self-scored ~58 on the card and ~72 after a figure redo, never independently reviewed. Below 80, so every technique here is a hypothesis, not a tested treatment.
**Layer:** Material (works on any subject) — warm/cool light and soft broken edges carry over; the people-centred warmth is the part that is hard to move to UI.
**Web fit:** Medium — editorial and marketing pages about people, hospitality, food, events, lifestyle brands; hero images and section backgrounds. Weak for dense product UI: dissolving edges and dappled light fight tables, forms and small text.

## Recognisable traits

- Warm skin and warm light set against cool, coloured shadows (blue-violet, never grey or black); the whole scene shares one light.
- Edges eaten by light: forms melt into their surroundings, soft and hard edges trade places, yet faces and hands are not broken into confetti.
- Dappled sunlight: warm yellow spots and cool violet shade patches scattered across ground, clothes and faces alike.
- Rounded, tactile bodies and fabrics; intimacy comes from people's relationship to each other, not from identical smiles.
- Visible, short, multi-directional strokes on ground and foliage; finer, calmer strokes where the eye rests (skin, hands).

Scope note from the source: this is the 1870s–early-1880s Impressionist Renoir. The more linear mid-1880s turn and the late work are different and should be labelled if used.

## Palette

Hexes come from the source study's reference notes (anchored on looking at the paintings, not official colours).

| Role | Hex | Note |
|---|---|---|
| Background (sunlit ground) | `#c8b890` → `#b0a07c` | Sandy garden floor; use as a warm page ground. |
| Surface (cream linen) | `#f4f0e4` | Dress trim / tablecloth white; cards and reading panels. |
| Text | `#2c3050` | Source "black" clothing is a deep blue-ink, never `#000`. |
| Muted text | `#4a4f82` | Lit side of the dark coat; 6.7:1 on cream. |
| Accent (light spot) | `#f6e4a0` | Warm dapple; highlights, hover glows, selection. |
| Accent 2 (cool shade) | `#6f78b8` | Violet-blue shadow; decorative only (3.6:1 on cream, fails body text). |
| Skin warmth | `#eab090` (+ flush `#c85a6a`) | Warm pink; avatars, illustrations, a soft CTA tint. |
| Fabrics | `#e8c8d0` pink, `#a8b8dc` powder blue, `#e6c070` straw yellow | Tag and chip colours. |
| Foliage | `#5f7f6a` → `#a8b890` | Canopy greens for headers or large backgrounds. |

Relationship: every light area leans warm (yellow-pink), every dark area leans cool (blue-violet); a shadow is a colour, not a darkening. No pure black, no grey shadows, no flat orange filter over everything. Light reading is natural. A dark reading (dusk garden: `#2c3050` ground, cream text, `#f6e4a0` lamp spots) is a suggested extrapolation; the source notes night scenes as a possible transfer test, not a proven one.

## Typography

A soft, humane serif for headings: **Cormorant Garamond 500–600** or **Fraunces** (soft optical size) — `"Cormorant Garamond", "Fraunces", Georgia, serif`. Body in a warm sans at 16–18px: **Source Sans 3** or **Nunito Sans**. Labels in small caps or +0.06em tracked uppercase in `#4a4f82`. Avoid script fonts: Renoir's softness is in colour and edge, not in lettering.

## Surfaces, texture and edges

- **Canvas weave:** a cached canvas-grain tile in `#ece2cc` laid over the page with `mix-blend-mode: multiply` at opacity ~0.20 (source value).
- **Broken-colour imagery:** don't run brushwork live. Re-paint hero art offline (or once at build time) with the broken-stroke method below and ship it as WebP/AVIF.
- **Dissolving edges:** cards and images get feathered, brushy edges with a `mask-image` made from a soft SVG of overlapping stroke blobs, not a hard `border-radius`. Keep the inner reading area fully opaque.
- **Dappled light overlay:** two pre-rendered transparent PNG layers of soft warm spots (`#f6e4a0`) and cool patches (`#6f78b8`), positioned over image areas and empty ground. On people or text, drop their opacity (source lesson: at full strength the light layer "cuts people in half").

## Shapes and layout

No hard rectangles at the edges of image blocks; inner UI can use small radii (6–10px). No outlines, no drop shadows with black: if a card needs lift, use a soft violet-tinted shadow (`0 10px 30px rgba(80,80,140,.18)`, suggested). Layout favours clusters of people and objects sharing one space: overlapping images, a dominant warm foreground and a cooler, looser background. Leave detail density higher where the eye should land (headline, face) and looser at the periphery.

## Components

- **Buttons:** cream or straw-yellow pill with ink-blue text; hover adds a warm light spot drifting across (`background-position` shift of a radial gradient).
- **Cards:** cream linen with a brush-feathered edge mask and violet-tinted shadow; image on top shares the same light colour.
- **Navigation:** quiet cream bar; active item marked by a warm dapple behind the text, not an underline.
- **Inputs:** cream field with a 1px `#4a4f82` hairline at 40%; focus ring is a warm `#e6c070` 2px outline (plus offset) so it stays visible.
- **Chat bubbles:** user bubble in powder blue `#a8b8dc`, replies on cream; both with soft feathered edges.
- **Tags:** fabric chips in pink, powder blue, straw.
- **Loading:** a patch of dappled light breathing (opacity cross-fade between two spot layers).
- **Empty state:** a single sunlit table or chair illustration, loosely painted, with one warm line of copy.

## Motion

- **Dapple sway (the source's motif):** two light layers cross-fade and drift: `q = 0.5 + 0.5·sin(1.3t)`, layer A alpha `0.55·(1−q) + 0.1`, layer B alpha `0.55·q + 0.1`; A drifts `(sin(0.7t)·10px, cos(0.6t)·6px)`, B the mirror. Reads as leaves moving overhead.
- **Gentle crowd sway:** illustrated figures lean ±0.05 rad at ~1.6 rad/s, each with its own phase.
- **Hover:** a warm light spot slides onto the element over 400–600ms, ease-out.
- **Done moment:** the item "steps into the sun" — a warm spot blooms behind it and the shade colour fades out.
- Easing is soft and smooth, never stepped. Under `prefers-reduced-motion`, freeze both light layers at their mid blend (`q = 0.5`) and skip drifts.

## Techniques

The source's `MEDIUM.broken` re-paint, usable offline in a canvas script to turn a clean illustration into strokes:

- Sample on a jittered grid (`cell` px), shuffle the samples deterministically, draw a tapered, slightly bent stroke at each one in the sampled colour, skipping ~2–10% so the base shows through. Second pass at 0.62× cell and 0.6× size.
- Colour jitter is complementary, not just brightness: where luminance > 0.55 nudge toward warm `rgb(255,226,160–170)`, otherwise toward cool `rgb(120–130,126–130,196–200)`; saturation ×1.1–1.12.
- Background (canopy and ground): `cell 7, len 22, wid 7, jit 30`; canopy angle `0.7 ± 0.35`, ground angle `0.08 ± 0.3`.
- Figures get their **own** pass with finer strokes and vertical-ish direction: `cell 4–5, len 12–15, wid 4–5, angle ≈ 1.25–1.3`. If people share the ground's horizontal strokes, they look like they are lying down (source lesson).
- Light layer: `cell 8, len 24, wid 8, angle −0.3`, no blurred base, so it's only spots.
- Before stroking, blur the base and over-draw it 14px past every edge, otherwise a row of half-transparent dots appears at the image border.

## Performance and accessibility

- Expensive: running the stroke re-paint in the browser (it reads every pixel). Do it at build time; ship images. The two light layers are just transformed PNGs — cheap. Pause drift when the tab is hidden.
- Contrast: ink-blue `#2c3050` on cream `#f4f0e4` is ~11:1. Violet `#6f78b8` and the fabric pastels fail for body text; never set text directly on broken-stroke imagery — put it on a cream panel.
- Motion: drifting light can bother some readers; keep amplitude ≤ 10px and honour reduced motion.

## Pitfalls

- People and background in one stroke pass → figures smeared sideways; re-paint each layer with its own direction.
- Light spots at full opacity over figures or text → faces cut in half; lower opacity over them.
- An orange filter over everything to make it "warm" → the source explicitly rejects this; warmth must alternate with cool shadow.
- Grey or black shadows → reads as generic photo filter; shadows are blue-violet.
- Doll-like figures and a too-orderly crowd (source shortcoming) → vary scale, overlap, and spacing; the original has pastel skin glow the study never achieved.

## Nearest neighbours

- **46 · Pissarro:** also broken colour, but Pissarro is about people inside an environment (roads, fields, work), with small, firmer, multi-directional strokes and orange-pink flecks in the greens. Renoir is about people themselves: softer edges, skin warmth, dappled light.
- **47 · Sisley:** Sisley's organising force is sky and weather, with long horizontal strokes and small figures. Renoir's is figures and warm light; sky barely matters.
- **08 · Impressionism (built):** the generic recipe uses fixed short pastel strokes and tends to break faces; Renoir keeps faces and hands calm and adds the warm/cool dapple system.

## Source

Adapted from huashu-art-motion · `45_renoir.md` (research recipe; self-score ~58 → ~72 after figure redo, not independently reviewed; known shortcomings: doll-like figures, background crowd too small and evenly spaced, missing the pastel skin glow of the originals).
