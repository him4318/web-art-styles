# 15 · Early ray-traced CGI (1990s computer graphics demos)

**Status:** Built ★★
**Layer:** Material (narrow use)
**Web fit:** Medium as a retro-tech easter egg, a launch page for a developer or graphics tool, or a nostalgic theme; Low for everyday product UI, where glossy plastic and lens flares fight with content.

## Recognisable traits

- Everything is shiny plastic: one shading convention for every object, with a hard white specular dot, a bright centre and darkened edges.
- Infinite checkerboard floors in perspective, with blurry reflections of the objects above them.
- Chrome spheres that mirror the floor and sky; glass tables with a sliding streak of reflection.
- Toy-like construction: limbs as cylinders, ball joints at shoulders and knees, wireframe-grid terrain on distant mountains.
- Lens flares: a glowing sun core, star rays, a long horizontal streak and a line of ring and hexagon ghosts.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (wall) | `#7dbcb9` → `#64a6a6` → `#3f8285` | Source radial gradient, lit from the window. |
| Surface | `#eef6f5` (suggested) | Glossy white panel for text. |
| Text | `#142a2c` (suggested) | Dark teal-black. |
| Environment tint | `rgba(70,150,150,.28)` | Source: the rim light every object picks up from the teal wall. |
| Edge shade | `rgba(10,0,30,.42)` | Source: dark purple at the rim of each plastic shape. |
| Sky (accent band) | `#a24cc4` → `#df62ae` → `#f5a08a` → `#f8c070` | Source sunset; use sparingly. |
| Accent | `#f8c070` (suggested from the sky) | Buttons, highlights. |

Relationship: a cool teal room with a warm sunset outside; every object carries the room colour in its rim light. Caution: the purple-pink sky and neon flares sit right in the "blue/purple gradient AI look" the source palette rules warn against. Keep the sunset to one hero band or a small window and let teal and warm sand carry the UI. A dark reading (suggested): a deep teal `#10282a` room with the same plastic shading.

## Typography

Display: a chunky early-90s tech face, suggested **Orbitron** 700 or **Michroma**, uppercase, with a chrome treatment (vertical gradient white → grey → dark, thin dark outline) for one headline only. Body: a clean sans (suggested **IBM Plex Sans** 400) on solid panels. Labels in **IBM Plex Mono** recall render logs and stats ("Rendered in 14h 32m").

## Surfaces, texture and edges

- **Plastic shading** (source `plastic()`), for buttons, chips, avatars and icons:
  1. base colour fill;
  2. radial gradient centred on the highlight point, radius R: 0 → white .55, 0.28 → white .06, 0.62 → transparent, 1 → `rgba(10,0,30,.42)` (rim darkening = volume);
  3. environment rim: an additive stroke `rgba(70,150,150,.28)`, width 0.18R, offset 0.04R to the left;
  4. one hard-edged white ellipse (0.16R × 0.09R, opacity .85) as the specular hotspot. This last mark is what reads as 90s Phong.
  In CSS this is a stack: `radial-gradient(...)` layers plus an `inset` box-shadow for the rim and a `::after` ellipse for the hotspot.
- **Cylinders** (bars, sliders, progress tracks): a linear gradient across the short axis: dark .25 → a narrow white .5 band at 22% → dark .45, plus a thin white highlight line from 20% to 75% of the length.
- **Checkerboard floor:** a perspective checkerboard behind a hero (`repeating-conic-gradient` on a plane with `transform: perspective(...) rotateX(...)`), fogged 35% toward the horizon. Pre-render it once as an image if you need anti-aliasing (the source supersamples 2×2).
- **Reflections:** mirror a hero object with `transform: scaleY(-1)` (or `-webkit-box-reflect`) at α .34, clipped to the floor. Render the mirrored group as one layer and fade it as a whole, or overlapping parts stack into opaque patches.

## Shapes and layout

Rounded, inflated forms: pill buttons, spheres, capsules. Soft edge darkening instead of outlines. Shadows are soft and offset away from one light source (source: window shadow blurred 10px toward bottom-right). Layout: a "scene" hero (floor + horizon + objects) and content on glossy panels floating above it; keep one consistent light direction for every highlight on the page.

## Components

- **Buttons:** plastic pills with the four-step shading; pressing moves the hotspot down and darkens the rim.
- **Cards:** glossy off-white panels with a thin teal rim light and a soft drop shadow; a translucent green "glass table" variant with a thick edge and a white streak.
- **Navigation:** a row of small plastic spheres or capsules; the active one is chrome (mirror the floor and sky in it).
- **Inputs:** recessed (inner shadow) plastic wells.
- **Progress / sliders:** cylinders with a ball-joint thumb.
- **Loading:** a ring of beads spinning about a vertical axis (see Motion), or a green wireframe that "renders" into a shaded shape scanline by scanline.
- **Empty state:** a single chrome sphere on a checkerboard.
- **Illustrations and mascots:** treat them as toys: smooth the outline first (angular polylines expose facets under glossy shading), use cylinders for limbs and balls for joints, print markings softly "under" the plastic with a 2px blur.

## Motion

- **Bead ring:** 140 beads on a torus spinning about a vertical axis at 7rad/s with 0.35 tilt, depth-sorted (a dark tube underneath so the beads join up, then per-bead highlights). For UI, slow it down (suggested 1–2rad/s) as a loader.
- **Chrome sphere:** the reflected checkerboard scrolls (`u += t · 4.5`), reading as rotation; Fresnel-bright rim. In CSS: animate `background-position` of a mirrored checker inside a circular mask.
- **Lens flare:** sun core glow 170px pulsing ±8% (23rad/s in the source, slow it for UI), 12 star rays rotating at 1.6rad/s, a 960px horizontal streak, 5 ghosts on the sun→centre line sliding by `sin(5t)·0.03`. Use it once, for a launch or "done" moment.
- **Glass streak:** a diagonal highlight sliding across glass at 260px/s; good as a hover sheen on cards.
- **Glow pulse:** an additive radial glow r 46px pulsing ±25% (the source's green cat-eye) as a "live" indicator.
- Easing: smooth and mechanical (linear rotations, ease-in-out slides). Under `prefers-reduced-motion`: stop rotations and flares, keep static highlights.

## Techniques

- Fake a renderer with shading conventions; no 3D engine needed. Cache everything static (floor, sky, mountains) once.
- Wireframe terrain: a 28×26 pink line grid over mountain silhouettes with zig-zag snow caps; an SVG pattern clipped to the silhouette.
- Chrome by environment mapping: in canvas, per pixel of a small sphere (104px in the source, under 2ms), the reflection vector picks floor checker (down) or sky plus a window streak (up). For DOM, fake it with a radial gradient: sky colours top, checker image bottom, bright rim.
- The "wireframe → render" reveal: compute edges from the fully rendered first frame, so that frame must be complete (no fade-in on the first frame).

## Performance and accessibility

- Expensive: per-frame environment maps, many depth-sorted beads, additive full-screen flares, perspective checkerboards with anti-aliasing. Cheap route: pre-render the floor and sphere frames, animate transforms, pause off-screen and in hidden tabs.
- Contrast: dark teal text on the glossy panel 13.7:1 and on the light wall `#7dbcb9` 7.0:1; white on the dark wall `#3f8285` is 4.4:1, which fails AA for body. Never set text on the sunset gradient or over a specular highlight.
- Flares and fast spins are motion-heavy; keep them short and off under reduced motion.

## Pitfalls

- Angular outlines under glossy shading show facets → smooth paths before shading.
- Reflections of overlapping parts each at α .34 add up to opaque → render the reflection group to one layer, fade the layer.
- Lens-flare streak at high opacity drowns the page → one flare per view, short.
- Blank or faded first frame before a wireframe reveal → the edge pass needs a complete frame.
- Purple-pink sky everywhere → reads as generic "AI gradient"; keep it to one band.
- Stiff toy figures (the source's straight "column" legs and mask-like face) → add bends and soft transitions, or keep figures small.

## Nearest neighbours

- **26 · Vaporwave:** shares the sunset, grid floor and retro-digital nostalgia, but uses neon glow lines and VHS noise on a dark ground. Ray-trace is daylit, glossy and volumetric, with checkerboards and chrome instead of neon.
- **53 · Pixar (research):** also 3D-looking, but aims for believable materials and acting. Ray-trace is deliberately one plastic material with hard specular dots.

## Source

Adapted from huashu-art-motion · `15_raytrace.md` (built ★★, narrow use; noted shortcoming: the cat's legs were straight columns and the face looked like a mask).
