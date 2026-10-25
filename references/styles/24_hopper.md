# 24 · Hard sunlight (Edward Hopper, American realism, 1920s–60s)

**Status:** Built ★★★
**Layer:** Period specimen — but the hard-edged slab of window light is a transferable lighting treatment that works on any subject.
**Web fit:** High for the light itself — a drifting slab of sun behind a product UI, a sweep across a finished card, quiet editorial pages. Medium for the full period look (green walls, red brick, empty rooms), which reads as homage. Weak where the page needs to feel busy or cheerful.

## Recognisable traits

- Sunlight arrives as hard-edged geometric slabs: parallelograms on walls and floors with crisp edges, cut by window mullions.
- Two temperatures only: cool, slightly green shadow and warm, yellow light. The same wall is two flat colours, not a gradient.
- Flat, quiet surfaces. Brushwork is barely there; walls read as planes.
- Empty space and stillness: long horizontals, few objects, figures (if any) caught in a pause.
- Small architectural specifics that make it real: window reveals with a hard shadow on one side, blinds pulled to uneven heights.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (shadow wall) | `#5d8679` | Source: cool green wall out of the light. |
| Background (lit wall) | `#efcf8f` | Source: the same wall inside the slab. |
| Surface / dado (shadow / lit) | `#4b6d63` / `#d8ad68` | Source: wainscot in both states. |
| Floor (shadow / lit) | `#5b3d2b` / `#c98b52` | Source. Good for a footer or bottom band. |
| Text | `#1f2a26` (suggested) | Near-black with a green cast; on light cards. |
| Card | `#f4ead6` (suggested) | Warm plaster for readable panels. |
| Accent | `#cc4a3c` | Source: lit red (dress); shadow pair `#7a2c2a`. Primary action. |
| Accent 2 | `#b4553a` | Source: brick across the street. |
| Gold (lit / shadow) | `#f3d67e` / `#a8893f` | Source: blond hair in both states; highlights, badges. |

Relationship: every colour exists as a pair, a shadow version and a lit version, and the slab decides which one shows. No blue sky gradients, no purple. Dark reading: night interior (think the diner paintings) with the slab as warm electric light on near-black green `#16201c` (suggested). Light reading: lit plaster everywhere, the slab becomes a slightly warmer, brighter band.

## Typography

Plain, mid-century and sober: **Libre Franklin** or **Public Sans** for UI (`"Libre Franklin", "Helvetica Neue", Arial, sans-serif`); for display, a restrained serif such as **Libre Caslon Text** or **Source Serif 4** at weight 400. Uppercase signage labels with +0.08em tracking can nod to storefront lettering. No script, no decorative faces: the drama is the light, not the type.

## Surfaces, texture and edges

- **Two-state rendering ("dual version clip"):** the core method. Build each surface twice, a shadow version and a lit version, and reveal the lit one only inside the moving slab polygon. On the web: a shadow-coloured layer underneath and a lit-coloured copy on top with `clip-path: polygon(...)`, or a single skewed band with `mix-blend-mode: screen`/`soft-light` over a cool ground. Clipping gives naturally hard edges and can be translated as one piece.
- **The slab:** a skewed rectangle (`transform: skewX(-20deg)`), split by a mullion (a 4% transparent gap in a hard-stop `linear-gradient`), with soft vertical fade via `mask-image` so it doesn't end in a box.
- **Mullion shadow:** a vertical bar about 24px wide plus one diagonal parallel to the slab's top edge.
- **Brush texture (optional, barely there):** vertically stretched noise (frequency ~0.06 horizontally, ~0.006 vertically), generated at quarter resolution and scaled up, `soft-light` at alpha 0.2. Pre-render it as a tile.
- **Window reveals:** a 10px hard shadow on the right side of every window opening; blinds at different heights (25–70%).

## Shapes and layout

Rectilinear: small radii (0–4px) or none. Shadows are hard and offset, never blurred halos — e.g. `box-shadow: 8px 10px 0 -3px rgba(0,0,0,.45)`. Layout leans on long horizontals and generous empty fields; let one quiet band of colour hold a lot of the page. Asymmetry: the light comes from one side (pick top-left or top-right and keep it for every element).

## Components

- **Buttons:** flat lit-red (`#cc4a3c`) or plaster with a hard offset shadow; on hover, the button "steps into the light" (switch to its lit colour pair).
- **Cards:** plaster panels with a hard offset shadow; important ones get a slab crossing a corner.
- **Navigation/sidebar:** the shadow-green wall as the panel; the active item is the lit-wall yellow with a 2px inset edge on the light side.
- **Inputs:** plaster field, dark 1px rule; on focus the field turns to its lit colour.
- **Chat bubbles:** user bubble in shadow tone with hard shadow; replies on plaster.
- **Tables:** thin rules, generous row height; no zebra striping (it fights the slab).
- **Loading:** a band of light sweeping across the running item, looping.
- **Empty state:** an empty wall with one slab of sun and a single line of copy. That is the Hopper image.
- **Illustration / mascot:** draw it twice too (shadow colours, then lit colours clipped to the slab) so the light falls across it, instead of a single flat sprite pasted on top.

## Motion

- **Slow sun drift (the motif):** the slab translates sideways very slowly, as if the sun were moving. Source moved it ~95px with an ease-in-out; on a page, 60–70s `alternate` cycles feel right (suggested). Transform only.
- **Done moment / attention:** one sweep of a skewed hard-edged band across the finished card, ~1.3s, `cubic-bezier(.45,0,.3,1)`. A running state loops the same sweep at ~3.2s.
- **Ambient extras:** dust motes visible only on the lit side of the slab edge (source: 26 motes); a blind cord swaying `sin(4.2t)·0.07rad`; a barber pole striping upward at 60px/s for a period easter egg.
- **A figure's shadow on the wall moves in sync with the figure:** for a mascot, offset its silhouette (source: +150, +18) and show it only inside the slab.
- `prefers-reduced-motion`: keep the slab where it is (the light is the style, not its movement), drop the drift, sweeps and motes.

## Techniques

```css
.slab {
  position: absolute; top: -15%; bottom: -15%; left: 52%;
  width: min(30vw, 420px); transform: skewX(-20deg); pointer-events: none;
  background: linear-gradient(90deg, var(--lit) 0 46%, transparent 46% 50%, var(--lit) 50% 100%);
  mask-image: linear-gradient(180deg, transparent 4%, #000 30%, #000 72%, transparent);
  animation: sun-drift 70s ease-in-out infinite alternate;
}
@keyframes sun-drift { from { transform: translateX(-18vw) skewX(-20deg) } to { transform: translateX(16vw) skewX(-20deg) } }
/* one-off sweep on an element */
[data-sweep]::after {
  content: ""; position: absolute; inset: 0; border-radius: inherit; pointer-events: none;
  background: linear-gradient(105deg, transparent 0 40%, rgba(255,222,165,.2) 40% 60%, transparent 60%) 100% 0 / 300% 100% no-repeat;
  animation: sweep 1.3s cubic-bezier(.45,0,.3,1) both;
}
@keyframes sweep { 70%, 100% { background-position: 0 0 } }
```

- Hard edges come from hard gradient stops (`40% 40%`), never from feathered gradients.
- To light an element whose layout stays fixed while it leaves a list (e.g. a task moving to Done), sweep a still copy held in place, then fade it upward.

## Performance and accessibility

- Cheap: one transformed div, gradient pseudo-elements, a pre-rendered brush tile. Avoid animating `clip-path` on large layers or `filter`; animate `transform` and `background-position` only.
- Contrast: never set body text directly on the shadow green `#5d8679` (white on it is ~4.1:1, fails for small text) — use plaster cards with dark text (~13:1). The slab must stay subtle over text (alpha ≤ 0.26) so text inside it keeps AA.
- Motion: the drift is slow enough to be calm, but turn it off under reduced motion.

## Pitfalls

- Horizontal brush noise at alpha 0.35 made walls look like watery marble → Hopper walls are flat; vertical streaks at ~0.2 or nothing.
- Soft-edged light → reads as a spotlight or Shinkai glow. Keep hard stops.
- Light coming from two directions on one page → pick one window and keep every sweep and shadow consistent with it.
- Key element left permanently in shadow (source: the cat stayed dark the whole time) → loneliness is a valid choice, but give the primary action at least a small patch of light.
- Editing code by pasting comments mid-line commented out the rest of the line (source note) → generic, but check batch edits.

## Nearest neighbours

- **35 · Shinkai light:** also sunlight as the subject, but Shinkai uses soft god rays, bloom, glowing particles and saturated skies; Hopper uses hard flat slabs, two-tone shadow/light and no glow.
- **32 · Rembrandt chiaroscuro:** also directional light, but Rembrandt is a dark multiplied light map with soft pools and lost edges; Hopper is daylight with geometric, crisp edges and a cool rather than black shadow.

## Source

Adapted from huashu-art-motion · `24_hopper.md` (built ★★★; noted shortcoming: the cat stayed in shadow throughout — consider a small patch of light from another window).
