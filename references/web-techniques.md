# Web techniques

How to build art-style treatments for real websites and apps: one table per medium, the "look" architecture that keeps styles switchable, and the performance and accessibility patterns that keep them usable. Style cards link here for shared methods; their own Techniques sections hold the style-specific parameters.

## The order of layers

Most painterly and print styles are built in this order:

1. **Flat base:** tokens, flat colours, layout. This fixes composition and hierarchy; the style isn't visible yet.
2. **Style pass on surfaces:** re-render backgrounds and panels in the medium (strokes, dots, mosaic tiles, halftone, pixels, gold leaf).
3. **Small things separately:** icons, avatars, badges and buttons get their own finer treatment. Passed through the same coarse renderer as a large background, they turn to mush.
4. **Illustrations and mascots:** flat fills, then the style's texture and edge, then a clear outline and features. They are where the eye lands, so they must read.
5. **Moving highlights:** the style's motifs and moments (loading, done, ambient loops).

Not every style follows this order:
- **Ink** reverses it: paper is the ground, ink is only ever added, lots stays empty.
- **Pattern styles** (Kusama dots, Bauhaus) are flat colour with pattern on top; the pattern *is* the renderer.
- **Chiaroscuro** (Rembrandt) paints a lit version, then multiplies a light map over it.

## Medium → web recipe

| Medium / renderer | Styles | Web recipe | Key points |
|---|---|---|---|
| Flow-field brushstrokes | 09 Van Gogh, 19 Munch, oil | Canvas drawn once: long curved strokes following a direction field per region, each with a darker edge. Cache as an image. | Regional palettes matter more than random colour jitter. Never redraw per frame. |
| Short colour dabs | 08 Impressionism, 45–47 | Canvas tile of warm and cool elliptical dabs at fixed positions; light spots can drift with `opacity`. | It is not pointillism: dabs are uneven and not pure colour. |
| Optical-mix dots | 29 Seurat | Pure-colour dots of equal size in proportion, plus complementary dots at the edges between light and dark. CSS: layered `radial-gradient` patterns. | Use a small pure palette. Dots can "breathe" with scale or opacity. |
| Mosaic | 04 Roman | SVG or canvas tiles laid along contours (distance-field rings), with grout from the gaps. Lay out once; animate colour flowing through the tiles. | Tiles 8–28% larger than half their spacing, so the bisectors cut clean grout lines. |
| Woodblock registration | Ukiyo-e, screen print | Fill offset from line art by (3, 2)px; patterns as tiled backgrounds; horizontal wash streaks. | The misregistration is the signature. Keep it consistent. |
| Facets | 11 Cubism, low-poly | Overlapping semi-transparent gradient polygons (`clip-path` or SVG) with straight-edged outlines. | Keep a dark, heavy field; facets alone read as "low-poly", not Cubism. |
| Halftone | 13 Pop, 22 Constructivism, old print | `background: radial-gradient(circle, ink 30%, transparent 31%) 0 0 / 6px 6px`, rotated 45°. Variable dot size by luminance needs canvas or SVG. | A small tile is ~10× cheaper than drawing each dot. |
| Pixel bitmaps | 14 8-bit | Sprites from string bitmaps as SVG `<rect>`s with `shape-rendering: crispEdges`, or a low-res canvas scaled with `image-rendering: pixelated`. Limit to ~14 colours with a 1px black outline per part. | Motion with `steps()` only. Pixel fonts for labels; a readable pixel-ish font (VT323) for body text. |
| Plastic and chrome shading | 15 Ray-traced CGI | Gradient "plastic" (base + radial dark rim + environment tint + hard highlight), checkerboard floors with mirrored reflections. | Niche. Good for a retro-tech hero, poor for long text. |
| Gold leaf | 05 Gothic, 18 Klimt | Pre-rendered brushed-gold texture with embossed lines (dark line plus 1px offset light line); a highlight sweep masked to gold areas only. | Animate the sweep with a moving gradient `mask`, not by re-rendering the texture. |
| Sfumato | 06 Renaissance | Soft blurred layer under a crisp semi-transparent layer; craquelure, canvas weave, vignette and a yellow varnish as one overlay tile. | Keep text off the craquelure. |
| Variable-width line | 10 Art Nouveau, calligraphy | SVG paths offset to polygons by width; a dark offset silhouette behind gives thick outside, thin inside. | Good for frames, dividers and headings. |
| Grain density | 16 2026 flat illustration | SVG `feTurbulence` tile thresholded to dots, masked with a gradient so grain thickens toward one corner. | Static. Bake it into the tile. |
| Dry brush and ink bleed | 17 Ink, 50 Ma Yuan | SVG filters: `feTurbulence` + `feDisplacementMap` + a speckled `feComposite` mask for dry-brush strokes; displacement + slight blur for washes. Strokes "write themselves" with `stroke-dashoffset`. | Paper is the ground; ink only darkens. Keep wide areas empty. |
| Long flowing strokes, row warp | 19 Munch | Canvas strokes 20–30 steps along a flow field; a sine warp per row as a static pre-render, or a slow CSS `transform` on a decorative layer only. | Never warp text. |
| Watercolour wash | 25 Ghibli backgrounds | Transparent layered fills with darker pooled edges: `::before` panels with an SVG displacement filter, plus `inset` box-shadows for the edge pooling. | Keep washes bright (low alpha). Heavy blooms read as mould. |
| Photo halftone | 22 Constructivism, newspaper | Dot size from luminance on a 45° grid, batched into one path or a canvas drawn once. | Red wedges and black circles do the rest. |
| Two-version light clipping | 24 Hopper, 23 Dalí | Lit and unlit versions of a panel; a hard-edged light shape clips between them, or a hard-edged light slab with `mix-blend-mode: screen` sweeps across. | Hard edges, slow movement, one slab at a time. |
| VHS channel split | 26 Vaporwave | RGB offset copies with `mix-blend-mode`, scanlines as a repeating gradient, a tracking-noise band moving occasionally. | The one sanctioned exception to the blue/purple rule. Keep text on solid panels. |
| Light map multiply | 32 Rembrandt | A lit version, then multiply a light map (window light plus candle radial gradients). | Faces and focal UI sit in the light; everything else falls off. |
| Cel shadow bands | 35 Shinkai | A shape minus itself shifted toward the light = a hard shadow band. On the web: an offset hard `box-shadow`, or a clipped pseudo-element, plus a lit rim. | Pair with breathing god rays and drifting dust. |
| Stepped frame rate | 33 Rubber-hose, 34 Shadow puppets | `animation-timing-function: steps(n)` or JS that updates at 10–12fps. | The judder is half the style, not lag. |
| Cut paper | 30 Matisse | Seeded jittered `clip-path` polygons on a `::before` "paper" layer, plus a pinned drop shadow. | Don't clip the interactive element itself (focus rings). |
| Rock relief and pigment | 01 Cave | `feTurbulence` + `feDiffuseLighting` rendered **once** to a half-size canvas and used as the background; pigment edges via a speckled displacement filter. | "Boiling" charcoal = switch between 2–3 precomputed states, or a CSS transform jitter, about 9 times a second. |

## The look architecture

To ship several styles in one product (as an optional "look", a theme pack or an easter egg), keep each one switchable and isolated.

- **One attribute:** `<html data-look="paper">`. Restore it before paint with a tiny inline script that reads `localStorage`, so the page never flashes the default look.
- **Scope everything:** `:root[data-look="paper"] [data-app] .card { … }`. The marketing site and other areas never change.
- **Hooks, not edits:** give shared components stable data attributes once, then style each look from its own file without touching shared code. For example: `data-sidebar`, `data-nav`, `data-bubble="user|agent"`, `data-live-status`, `data-tool="running|done|error"`, `data-composer`, `data-action="open|done"`, `data-mascot` + `data-mood`.
- **One CSS file per look** (`styles/looks/<id>.css`).
- **An effects layer per look:** a lazily loaded component that renders fixed, `pointer-events: none` overlays (rays, dust, embers, clouds, sprites). Load it only while that look is on, and clean up listeners and animation loops on unmount.
- **Moments as events:** components announce `task-done` (with the element) or `turn-done`; each look's layer plays its own effect (sticker, light sweep, hand print, coin and "+1"). Emit the event before the network request, then re-read server state after it settles.
- **Fonts per look:** inject the Google Fonts `<link>` when the look turns on and remove it when it turns off. Expect a brief fallback font the first time; size the fallback so the layout doesn't jump.
- **Both themes:** each look defines its own tokens under light and dark (for example dusk vs morning light, a night level vs a day sky). Never force a single theme on users who chose the other.
- **Collapsed or mobile chrome:** check the narrow icon rail and the mobile drawer. Translucent panels that look good on desktop often need to go solid in a drawer.

## Recipes

### Pinned paper card

```css
.card { position: relative; isolation: isolate; }
.card::before {           /* the paper */
  content: ""; position: absolute; inset: 0; z-index: -1;
  background: var(--paper);
  clip-path: var(--cut);  /* set per element from a seeded polygon */
}
.card { filter: drop-shadow(5px 6px 3px rgb(40 25 10 / .26)); }
```

### Ink wash bubble and dry-brush line

```html
<svg width="0" height="0" style="position:absolute" aria-hidden="true"><defs>
  <filter id="bleed" x="-20%" y="-20%" width="140%" height="140%">
    <feTurbulence type="fractalNoise" baseFrequency=".04" numOctaves="3" seed="5" result="n"/>
    <feDisplacementMap in="SourceGraphic" in2="n" scale="9"/>
    <feGaussianBlur stdDeviation="1.2"/>
  </filter>
  <filter id="dry" x="-10%" y="-40%" width="120%" height="180%">
    <feTurbulence type="fractalNoise" baseFrequency=".9 .06" numOctaves="2" seed="11" result="n"/>
    <feDisplacementMap in="SourceGraphic" in2="n" scale="3" result="d"/>
    <feTurbulence type="fractalNoise" baseFrequency=".5 .03" seed="3" result="m"/>
    <feColorMatrix in="m" values="0 0 0 0 0  0 0 0 0 0  0 0 0 0 0  0 0 0 -3 2.1" result="mask"/>
    <feComposite in="d" in2="mask" operator="in"/>
  </filter>
</defs></svg>
```

```css
.bubble::before { content:""; position:absolute; inset:0; z-index:-1;
  background: rgb(139 129 114 / .16); border-radius: 18px 26px 22px 30px; filter: url(#bleed); }
.loader path { stroke-dasharray: 160; stroke-dashoffset: 160; filter: url(#dry);
  animation: write 2.2s cubic-bezier(.6,.1,.3,1) infinite; }
@keyframes write { 70%, 100% { stroke-dashoffset: 0; } }
```

### God rays and a hard sunlight slab

```css
.rays { position: fixed; inset: -20% -10% 20% -30%; pointer-events: none; mix-blend-mode: screen; filter: blur(14px);
  background: repeating-conic-gradient(from 100deg at 8% 0%, rgb(255 196 120 / .13) 0 3deg, transparent 3deg 9deg);
  mask-image: radial-gradient(70% 70% at 10% 0%, #000 20%, transparent 75%);
  animation: breathe 7s ease-in-out infinite; }      /* opacity only; never animate the blur */
@keyframes breathe { 50% { opacity: .55; } }
.slab { position: absolute; inset: -20% auto -20% -40%; width: 34%; transform: skewX(-22deg);
  background: linear-gradient(90deg, transparent, rgb(255 214 150 / .14) 8% 92%, transparent);
  mix-blend-mode: screen; animation: sweep 6s cubic-bezier(.45,0,.4,1) infinite; }
@keyframes sweep { 60%, 100% { left: 120%; } }
```

### Pixel sprite from a string bitmap

```js
const PAL = { k: "#000", q: "#f8b800", w: "#fcfcfc", b: "#ac7c00" };
const COIN = [".kkkk.", "kqqwqk", "kqwqqk", "kqwqqk", "kqwqqk", "kqqqqk", ".kkkk."];
function sprite(rows, px = 3) {
  const rects = rows.flatMap((row, y) => [...row].map((c, x) =>
    PAL[c] ? `<rect x="${x}" y="${y}" width="1" height="1" fill="${PAL[c]}"/>` : "")).join("");
  const w = Math.max(...rows.map(r => r.length)), h = rows.length;
  return `<svg viewBox="0 0 ${w} ${h}" width="${w * px}" height="${h * px}" shape-rendering="crispEdges">${rects}</svg>`;
}
```

### Draw an expensive texture once

```js
// Rasterise a heavy SVG filter (e.g. lit rock) once, at half size, then reuse it as a background.
async function bake(svgMarkup, w, h) {
  const img = new Image();
  img.src = "data:image/svg+xml," + encodeURIComponent(svgMarkup);
  await img.decode();
  const c = Object.assign(document.createElement("canvas"), { width: w / 2, height: h / 2 });
  c.getContext("2d").drawImage(img, 0, 0, c.width, c.height);
  return c.toDataURL("image/webp", 0.85);  // set as background-image; redo only on debounced resize or theme change
}
```

### Particles (dust, embers) that stay cheap

- Pre-draw one sprite per particle size and `drawImage` it. Don't create a gradient per particle per frame.
- Use 18–56 particles depending on screen size, with `devicePixelRatio` capped at 2.
- Stop the loop when `document.hidden`, and draw a single still frame under reduced motion.

## Accessibility and performance patterns

- **Readable panels over textures:** put text on a calm surface (paper, chalk, a dialog box) rather than directly on rock, gold or halftone. Measure contrast at the worst spot under the text.
- **Keep colour and texture apart:** `background-color: var(--panel); background-image: var(--texture, none);`. A `background` shorthand containing an unset variable makes the *whole* declaration invalid, so the panel loses its colour too.
- **Reduced motion:** `@media (prefers-reduced-motion: reduce) { .look * { animation: none !important; } }`, plus a JS check that skips animation loops and draws one still frame.
- **Focus:** a visible focus style in the look's language (a pixel outline, an ink ring, a paper tab). Never `outline: none` without a replacement.
- **Hit areas:** decorative overlays get `pointer-events: none` and a z-index that never covers controls; tap targets stay at least 44px on touch.
- **Composited motion:** animate `transform` and `opacity`. Animating `filter`, `box-shadow`, `background-position` on large areas, or blur radius, is expensive.
- **Stable layout:** reserve space for webfonts and baked textures so nothing jumps when they arrive.
- **Dev servers can serve stale CSS:** some bundler dev caches keep old CSS after edits. If a change doesn't appear, clear the dev cache before debugging the CSS.

## Source

The medium table adapts huashu-art-motion · `references/04-纯代码绘制.md` (renderer quick reference) for the web. The look architecture, recipes and pitfalls come from building four of these styles (cut paper, cinematic light, cave painting, 8-bit) as switchable looks in a production Next.js app.
