# 46 · Pissarro (Camille Pissarro, Impressionism, 1870s–1890s)

**Status:** Research recipe — one redone single-scene study (a market-day country road), self-scored ~75, independently reviewed ~58. Below 80; techniques are hypotheses.
**Layer:** Material (works on any subject) — surface-following strokes and "life inside an environment" transfer to any scene with paths, places and activity.
**Web fit:** Medium — editorial and marketing pages about places, communities, local business, logistics, maps and journeys; illustrated section headers. Weak for dense product UI: busy textured surfaces compete with data.

## Recognisable traits

- Small, directional strokes that *build* the surfaces: roads, walls, fields and foliage each have their own stroke direction. Not random noise.
- Weather and time of day unify all local colour; shadows still carry reflected environmental colour.
- People are participants in daily life with a place, a scale and a task (carrying, pushing, walking), never decorative dots.
- A road or street that leads the eye into depth; viewpoint can be high (boulevards seen from a window) or at ground level.
- In the greens: warm orange-pink flecks jumping against cool green — the study's main way of telling it apart from Sisley.

Scope note from the source: keep the Neo-Impressionist (pointillist) phase separate; don't mix dots and loose strokes on one page.

## Palette

Hexes from the source study (sky, fields, figures); UI roles are suggested mappings.

| Role | Hex | Note |
|---|---|---|
| Background (pale horizon) | `#efe3c8` | Warm cream where sky meets land; page ground. |
| Surface | `#f6efdf` (suggested) | Slightly lighter card paper. |
| Text | `#4a3a2a` | Source dark brown (harness, staffs); 8.5:1 on the ground. |
| Muted text | `#6b5a4a` | Tree-trunk brown. |
| Sky | `#7fa6d2` → `#b4cadc` | Header bands, hero top. |
| Accent (field green) | `#5f8a42` / `#86a850` | Large blocks and success; 3.2:1 on cream, not for small text. |
| Accent 2 (warm fleck) | `#d6a84a` / `#e9b6b8` | Ochre and pink flecks inside green; badges, highlights. |
| Road | `#c9a778` | Path/progress lines. |
| Clothes | `#5a78a8` blue, `#a8584a` brick, `#6f7f4a` olive | Avatars, category colours. |

Relationship: greens are never plain — always shot through with warm ochre/pink flecks; lights lean warm `rgb(255,214–216,140–150)`, darks lean cool `rgb(110–130,120–130,190)`. No black. Dark reading (suggested): a winter dusk street — `#3a3a4a` ground, cream text, ochre lamp flecks.

## Typography

A sturdy, unfussy serif with some rural character for headings: **Libre Caslon Text** or **Lora 600** — `"Libre Caslon Text", "Lora", Georgia, serif`. Body in a humanist sans (**Source Sans 3**, 16–18px). Labels as small, tracked caps in olive or brown. Keep type plain; the energy belongs in the surfaces.

## Surfaces, texture and edges

- **Painted surfaces with direction:** illustrated backgrounds re-painted offline with strokes whose angle follows the surface — sky near-horizontal (−0.12), the road along its own perspective (−0.75), fields gently sloped (0.1 ± 0.2), foliage multi-directional (0.9 ± 1.1). Ship as images.
- **Canvas grain:** cached tile in `#e8e0cc`, `multiply` at ~0.20.
- **Card edges:** straight but softly broken — a narrow brush-stroke mask (6–10px deep) on image edges; text panels stay clean rectangles with a 4px radius.
- **Path motif:** a winding road (`#c9a778`) as a decorative SVG path that leads from hero to content, doubling as a progress line.

## Shapes and layout

Gentle radii (2–6px). Thin brown hairlines (1px at 30%) for dividers. Shadows warm and low (`0 6px 18px rgba(74,58,42,.15)`, suggested). Layout tends toward a clear perspective path: content placed along a diagonal that leads into depth, with small "activity" vignettes (illustrations of people doing things) beside the text. Don't fill empty areas with crowds; density should come from the story (source rule).

## Components

- **Buttons:** olive or brick fill with cream text, or cream with brown text; hover adds a few warm flecks (an overlay of stroke marks).
- **Cards:** cream paper with a small painted vignette at the top (a person doing the task the card is about).
- **Navigation:** a horizontal band in sky blue strokes; active item gets an ochre stroke underline drawn as a short brush mark.
- **Inputs:** cream field, brown hairline; focus = 2px `#5a78a8` outline.
- **Tables:** clean; alternating rows in very light field green (`#5f8a42` at 6%).
- **Badges:** small fleck-coloured chips (ochre, pink, olive).
- **Loading:** a tiny figure walking along a road line (looped 2-frame step), or flecks drifting off a tree.
- **Empty state:** an empty road leading into the distance, one line of copy.
- **Maps/timelines:** the strongest native component — routes as painted roads, stops as small activity scenes.

## Motion

- **Foliage flecks (source motif):** a few small coloured strokes float off tree crowns: `x + sin(1.6t + φ)·5px`, `y + cos(1.9t + φ)·3px`, colours from `#e9b6b8, #b8c060, #f2d2c4, #86a850, #d6a84a`, 6px wide, alpha 0.85.
- **Walkers:** figures walk toward the viewer along the road, step `sin(5t + φ)` with a small bob (`|step|·3px`) — a good loader or onboarding motif.
- **Clouds:** slow horizontal drift of a pre-painted sky strip.
- Smooth easing, unhurried but not slow-motion (source: a quiet look is not a reason for slow motion). Under `prefers-reduced-motion`, stop flecks and walkers, show the still painting.

## Techniques

Offline stroke re-paint (same engine as Renoir/Sisley, different settings):

- Base: `cell 8–9, len 22–34, wid 7, passes 2, jit 20–34, skip 0.04–0.06`, complementary jitter warm `[255,214,140]` / cool `[130,120,190]`, saturation ×1.08–1.12.
- Tree crowns: before stroking, fill each crown with ~70 small ellipses from a mixed palette (`#86a850, #b8c060, #d6a84a, #5f8a42, #e0c070, #4d6b3a`; blossom variant with pinks), darken the underside, lighten the top with `#f0d878`, then stroke multi-directionally.
- Moving figures: draw each on a small offscreen canvas, re-paint it with fine strokes (`cell 5, len 12, wid 4.5`), and composite back; cache per animation frame of a short loop rather than per render.
- On the web, figure loops can be a 6–8 frame WebP sprite stepped with `steps()`, which keeps the painted texture stable.

## Performance and accessibility

- Expensive: live stroke re-painting; per-frame figure painting. Cheap: pre-rendered backgrounds, sprite loops, transformed fleck elements. Pause when hidden; cap DPR at 2.
- Contrast: brown `#4a3a2a` on cream ~8.5:1. Field green on cream is 3.2:1 and blue clothes `#5a78a8` 3.5:1 — large text or decoration only. Never put text over painted fields; use cream panels.
- Busy textures can be tiring: keep stroke texture out of reading areas.

## Pitfalls

- Tree crowns read as cauliflower or "fuzzballs", trunks like palm trees (source shortcoming) → let branches show through, vary crown silhouettes.
- A near tree hides the landmark (the study had to remove one) → keep the focal building or headline clear of foreground objects.
- Faces as white blobs (source shortcoming) → at small scale use a simple warm tone with a shadow side, not a white disc.
- Adding people to hide an empty composition → density must come from the story.
- Confusing it with Sisley → shorter, multi-directional strokes and warm flecks in greens; keep activity on the page.

## Nearest neighbours

- **47 · Sisley:** Sisley's strokes are long and horizontal, the sky dominates and people are tiny silhouettes. Pissarro's strokes are short and multi-directional, the ground and road dominate, and people do visible work.
- **45 · Renoir:** Renoir centres people, skin warmth and dappled light with soft edges. Pissarro keeps people at environmental scale and gives surfaces firmer, more structural strokes.
- **29 · Seurat (built):** dots of pure colour on a regular grid. Pissarro (in this recipe) is loose strokes; keep his own pointillist phase separate.

## Source

Adapted from huashu-art-motion · `46_pissarro.md` (research recipe; self ~75, independent ~58; known shortcomings: cauliflower crowns and palm-like trunks, blank faces, Sisley distinction still unverified side by side).
