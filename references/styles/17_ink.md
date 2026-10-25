# 17 · Chinese ink painting, xieyi (Bada Shanren, Qi Baishi; 17th–20th century)

**Status:** Built ★★
**Layer:** Material (works on any subject)
**Web fit:** High for editorial pages, writing and reading tools, calm assistants, culture and food brands; Medium for product UI (great chrome and moments, keep tables plain). Low where a dense, colourful interface is needed: the style lives on empty paper.
**Mockup:** `assets/mockups/17_ink.png` (shown with 50 · Ma Yuan)

## Recognisable traits

- Paper is the ground and ink only adds: large areas stay empty (*liubai*, "left white"), and the empty space is part of the composition.
- Brush strokes with pressure: a swelling head, a body that thins as ink runs out, and "flying white" (feibai) where dry bristles skip and paper shows through.
- Ink washes that bleed into the paper with soft halos; tones of a single ink range from burnt black to the palest grey ("five colours of ink").
- Few colours, used sparingly: ochre, indigo-blue, rattan yellow, and a few touches of red (a sun, lips, a seal).
- A red seal stamp (a square with carved characters) placed off to one side as a signature.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (xuan paper) | `#efe6d0` (source) / `#efe7d4` (mockup) | Warm rice paper with fibres and a vignette. |
| Text (ink) | `#1d1814` (mockup); source ink `rgb(16,14,12)` | Body text and strokes. |
| Ink tones | ink at α .9 / .8 / .6 / .35 / .15 | Source "five colours": burnt, thick, heavy, light, clear. |
| Muted text | `#6b6052` (suggested) | 5.0:1 on paper. The mockup's wash `#8b8172` is decoration only (3.1:1). |
| Seal red | `#c4281e` (source) / `#b3261e` (mockup) | Seal, one key action, live indicator. |
| Ochre | `#c47a3c` | Warm washes. |
| Indigo (huaqing) | `#6f8aa0` / `#7d97ab` | Cool washes. |
| Vermilion sun | `#d8553f` | One focal accent. |

Relationship: ink on paper first; colour is a thin, transparent wash laid over (multiply), never an opaque block. Red appears in at most two or three small places per view. Dark reading (suggested): a sumi-black ground `#16130f` with paper-coloured "reverse ink" text and the seal kept red; it reads as night, not as the classical style, so the light reading is primary.

## Typography

Headings: an elegant serif with calligraphic contrast, suggested **Cormorant Garamond** or **Noto Serif SC/TC** for CJK (`"Cormorant Garamond", "Noto Serif SC", Georgia, serif`), regular weight, italics for asides (the mockup uses serif italics for the user's message). Body: a quiet serif or humanist sans at normal weight, generous line-height (1.6–1.7). A brush font (suggested **Ma Shan Zheng** or **Zhi Mang Xing**) only for one or two characters, e.g. inside the seal. Numbers and accents in seal red.

## Surfaces, texture and edges

- **Paper:** a warm fill plus a fibre texture and a vignette. The mockup does it with a 200×200 SVG `feTurbulence` stretched along one axis (`baseFrequency=".012 .25"`, 3 octaves) coloured brown at low alpha, under `radial-gradient(120% 90% at 50% 40%, transparent 55%, rgba(80,60,30,.18))`. The source adds 2,600 short fibre arcs (half brown α .05–.13, half light α .12–.27) and a vignette reaching α .22 at the edges: pre-render that as one tile.
- **Bleeding ink (SVG filter `#bleed`):** `feTurbulence` (fractalNoise, .04, 3 octaves) → `feDisplacementMap` scale 9 → `feGaussianBlur` 1.2. Apply to wash shapes, dots, soft pill backgrounds behind messages.
- **Dry brush (filter `#dry`):** displace with anisotropic noise (`.9 .06`, scale 3), then mask with a second thresholded noise (`feColorMatrix` alpha `-3 2.1`) composited `in`, so the stroke breaks up into flying white. Apply to rules, borders and loader strokes.
- **Washes:** soft irregular shapes (`border-radius: 40% 55% 45% 50%`) in wash colour at α .16–.28 through `#bleed`, laid behind text blocks instead of cards with borders.
- **Leave white:** instead of painting white details, cut them out of a wash (`mask` or `destination-out` in canvas) so paper shows through.

## Shapes and layout

No boxes. Structure comes from brush rules (a single dry-brush vertical line as a sidebar divider, a horizontal stroke above the composer), from washes and from space. Asymmetric composition: weight low and to one side, air on the other (a faint wash mountain in an empty corner at α .22). Corners are soft or nonexistent; no shadows. Leave at least a third of the view empty.

## Components

- **Buttons:** text in ink with a brush underline; primary as an ink-wash pill through `#bleed`; one critical action may be seal red.
- **Cards / replies:** a pale wash behind the content (`rgba(139,129,114,.16)`, organic radius, bleed filter); serif heading, plain body.
- **Navigation:** text items; the active one gets a small bled ink dot to its left and a heavier weight; counts in seal red.
- **Inputs / composer:** no box, just a 1.5px ink line on top through the dry filter.
- **Tables:** hairline rules in ink at 14% opacity; nothing else.
- **Checkboxes:** a 1.5px ink circle through bleed; done items fade to wash colour.
- **Badges / tags:** wash-coloured text with a faint ink border.
- **Status:** completed steps prefixed with an ink "—", the running step with a seal-red pulsing dot.
- **Loading:** an ink stroke writing itself (dash offset), see Motion.
- **Seal:** a 52px red square with paper-coloured carved text (inset double line), rotated −8°, stamped onto a finished answer.
- **Mascot / logo:** an ensō circle drawn once in dry brush.

## Motion

- **Strokes write themselves:** each stroke reveals along its length with `stroke-dasharray` and `stroke-dashoffset` (mockup loader: 8px round-cap stroke, 2.2s `cubic-bezier(.6,.1,.3,1)` loop). The source writes strokes in order over the first 0.45s and starts with a few strokes already down, because an empty first frame looks broken.
- **Wash spreads:** a wash grows from nothing then keeps widening slowly: halo blur `halo·(0.4 + grow)`, body blur `blur·(1.6 − 0.6·grow)`. In CSS: animate `opacity` and `transform: scale` of a pre-blurred wash, not the blur radius.
- **Seal stamp:** from 1.5–1.9× scale to 1×, overshoot ease (mockup: 0.45s `cubic-bezier(.2,1.5,.4,1)`), multiplied onto the paper. Use it as the "done" moment.
- **Ambient:** bamboo swaying `sin(2.9t)·0.11rad` from the base, more at the tips, leaves flicking `sin(7t + g)·0.2`; mist bands (paper-coloured blurred ellipses, blur 14, α .72) drifting 150–270px/s across a distant wash; a two-stroke bird crossing quickly (~8 wingbeats per second). Keep any of these slow and peripheral in UI.
- Easing is calm, ease-out; nothing bounces except the seal. Under `prefers-reduced-motion`: show strokes fully drawn, the seal already stamped, no mist or sway.

## Techniques

- **Brush model (canvas):** densify points with Catmull-Rom, resample every 4px of arc length (otherwise the flying-white noise frequency changes with stroke length). Pressure `prof(q) = smoothstep(0, head, q) · (1 − 0.7·smoothstep(1 − tail, 1, q)) · (0.85 + 0.15·noise)`. A wet core ribbon with width `w·prof·(1 − 0.85·dry·q^0.8)` at α tone×0.72, plus bristles: each a continuous polyline offset ±0.475w·prof, broken where `noise(k·1.9, i·0.09) < dry·q^1.2·0.95 + 0.05`.
- Source stroke parameters at 1920px: frame lines w14–18, dry .45–.6; a thick table edge w22; drapery w4–6, dry .3–.4, tone .78; a ground line w5, dry .85, tone .35.
- **For web:** render brush strokes once to SVG paths or PNGs (logo, dividers, empty-state art) and reuse them; for UI chrome the two SVG filters above are enough.
- **Wash under layers:** multiply washes are transparent, so lines behind show through. Fill the silhouette with paper colour first, then the wash: "reserve the space on the paper".

## Performance and accessibility

- Expensive: SVG displacement filters on large or animated areas; thousands of bristle paths per frame. Cheap: filters on small elements, brush art pre-rendered, paper as a cached tile. Never animate a filter's parameters.
- Contrast: ink on paper 14.3:1; seal red on paper 5.3:1 (fine for short labels); paper on the seal 4.6:1 (large text only). The wash grey `#8b8172` is 3.1:1, so use `#6b6052` for muted text.
- Filters on text blur letter edges: never apply `#bleed` or `#dry` to body copy, only to backgrounds, rules and decoration.

## Pitfalls

- Bristles drawn as separate short segments with round caps → beads at the joins look like bamboo or rope; one continuous path per bristle, butt caps.
- Drawing strokes live every frame → thousands of paths; draw once, cache.
- A decorative bird hidden in leaves → move motion into empty space where it reads.
- Transparent washes letting lines show through figures → read as wrong layering; paper-fill the silhouette first.
- A blank first frame → start with a few strokes already down.
- Figures drawn as neat outlines with flat colour → not xieyi enough (the source's own shortcoming); suggest form with few loose strokes and washes.

## Nearest neighbours

- **50 · Ma Yuan (research):** same ink tradition, but Southern Song: refined lines, a strongly off-centre "one corner" composition and vast mist. Xieyi is looser and bolder, with spontaneous strokes and splashed washes.
- **34 · Shadow puppet:** also Chinese, but translucent coloured leather silhouettes lit from behind, with jointed stepped motion. Ink is monochrome on paper and moves by writing and spreading.

## Source

Adapted from huashu-art-motion · `17_ink.md` (built ★★; noted shortcoming: figures not xieyi enough, review 6/10). Web filters come from the "ink & seal" mockup (`assets/mockups/17_ink.html`).
