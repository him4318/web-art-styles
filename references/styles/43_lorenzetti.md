# 43 · Lorenzetti (Ambrogio Lorenzetti, Sienese fresco, 1330s)

**Status:** Research recipe — reworked three times against two works (*Effects of Good Government in the City*, *Allegory of Good Government*); self-score ~72, not independently reviewed; below the catalogue's 80 bar. Techniques are hypotheses tested in one scene.
**Layer:** Period specimen (strongest as tribute) — but its core idea, a readable social space where many activities connect, transfers well to explanatory pages.
**Web fit:** Medium — illustrated overview pages, "how it works" flows, community/civic/urbanism sites, marketplaces, an illustrated map or sitemap of a product. Weak as a skin for dense UI chrome; strong as one large explanatory illustration with UI around it.

## Recognisable traits

- A city as a readable world: buildings stacked one above another filling the upper half, roads, doorways and arcades linking activities.
- Many small figures, each clearly doing a job (shoemaking, carrying, building, dancing, leading a mule), with a visible main path and cause-and-effect.
- Pink, grey and ochre walls with rows of small arched windows, crenellated towers, ground-floor arcades cut open to show workshops.
- Earth and mineral colours unified under one fresco surface; a dark brown painted border band.
- Local viewpoints that serve reading rather than one strict perspective.

## Palette

| Role | Hex | Note |
|---|---|---|
| Background (plaster) | `#efe3d0` (suggested) | Light fresco ground. |
| Surface / panel | `#f6eee0` (suggested) | Cards. |
| Text | `#2a2420` (suggested) | ~12.1:1 on plaster. |
| Muted text | `#6b5a48` (suggested) | ~5:1. |
| Wall pink | `#e3c3b4` (suggested) | Building fills, section backgrounds. |
| Wall grey | `#b9b2a6` (suggested) | Neutral blocks, dividers. |
| Ochre | `#c99a5b` (suggested) | Secondary fills, highlights. |
| Accent (Sienese red) | `#a8473a` (suggested) | Doors, banners, primary CTA. |
| Green (fields, success) | `#6d7f5a` (suggested) | Landscape, success. |
| Border band (dark brown) | `#4a3426` (suggested) | Frames, footer, nav; cream text on it ~9.7:1. |

All suggested from the source's description (pink/grey/ochre walls, dark brown border). Relationship: warm earths and soft pinks unified by plaster; the source warns against an "old paper" filter that darkens everything and kills information. Dark reading: the brown border band as page ground (`#4a3426`) with plaster cards is plausible for a footer or night section.

## Typography

Gothic-era but readable: blackletter faces (e.g. **UnifrakturMaguntia**) are too dark and too heavy for UI — prefer **Cormorant SC** or **Alegreya SC** for headings; body in **Alegreya** or **Crimson Pro**. Small labels on the illustration (shop signs, street names) in small caps on banners. Inscription band text in cream on the brown border, uppercase, +0.1em.

## Surfaces, texture and edges

- **Fresco surface:** a plaster multiply tile plus a light veil over the whole illustration (shared with 38 · Giotto). Don't stain it with an aged-paper filter.
- **Building blocks:** each building as a flat-faced volume: lit front in the wall colour, side face one step darker, rows of small arched windows (an SVG `<pattern>` of arch-topped slots), crenellations along roof lines (`clip-path` with square teeth or a stepped path).
- **Cut-open arcades:** a building with its ground floor removed to reveal a scene inside — a natural frame for a feature card or a screenshot.
- **Border band:** a dark brown frame around the main illustration or the page, with a thin ochre inner line.

## Shapes and layout

Square corners; no drop shadows; depth by stacking and side faces. Layout as a *readable space*: one large illustration where each "workshop" or doorway is a hotspot for a feature, linked by a visible street (the path). Around it, a clean grid of cards that mirrors the hotspots. Order matters: show a main route and secondary activities, not a row of unrelated tiles.

## Components

- **Buttons:** Sienese red or ochre fills with dark or cream text, square, like a painted door; hover opens a shadow inside (the doorway).
- **Cards:** plaster panels shaped like building fronts (small arch row at the top edge as decoration).
- **Navigation/sidebar:** a horizontal brown border band with inscription-style labels; or each nav item as a tower/building with its name on a banner.
- **Inputs:** plaster fields with a brown bottom rule.
- **Chat bubbles:** banner-like panels; avatars as small figures with a clear job (tool, book, basket).
- **Tables:** fine — rows like building floors, alternate wall tints.
- **Badges/tags:** small flags/banners in red and ochre.
- **Illustrations:** small figures with long robes in 3/4 view, small heads, one front foot showing under the hem, light on one side; each holding the tool of their task.
- **Loading:** a figure walking along the street, or a block being hoisted on a scaffold.
- **Empty state:** an empty open workshop with the shutter up.

## Motion

- Life in the street: a loop of small figures walking (near larger, far smaller), a ring dance turning, a mule plodding, a stone being hoisted, a shoemaker hammering, a flag waving.
- Calm, continuous loops; each motion small and local. Figures redrawn on top of the static, pre-rendered city.
- Hover over a hotspot: its figures animate (e.g. the shoemaker starts working).
- Done moment: a banner unfurls over the completed item.
- `prefers-reduced-motion`: a still city; hotspots indicate state with outlines, not motion.

## Techniques

- **Build order (source):** sky → low mid-ground buildings → left/right main buildings → cut-open workshop → street → steps; apply the fresco medium; draw small figures every frame on top. On the web: pre-render the city as SVG/PNG layers and animate only figures (CSS or small sprites).
- **Hotspots:** an SVG overlay with `<a>` regions over each workshop, focusable and labelled — the illustration becomes navigation.
- **Paths first:** plan the connections between places (who goes where), then place figures along visible routes.

## Performance and accessibility

- Cheap: static city layers + a handful of animated sprites. Expensive: dozens of SVG figures animating with filters — keep figures filter-free; cap the crowd (the source used ~20 walkers).
- Contrast: umber text on plaster ~12:1, cream on brown ~9.7:1. Labels inside the illustration need their own solid banner backing.
- Hotspots must have accessible names and keyboard focus; don't make the picture the only path to content.
- Busy crowds can distract → keep motion confined to the illustration and off under reduced motion.

## Pitfalls

- Blank plaster gaps in the middle of the city (fixed in round 2) → fill the composition edge to edge with buildings.
- Figures spreading both arms like cartoons → restrained gestures, long robes, 3/4 views.
- Overlapping actors (a mule over the dancers) → plan paths and depth order.
- An old-paper filter over everything → keep colours clear and information readable.
- Too few people, flat walls, no receding street (noted shortcomings) → more crowd, painted wall texture, a street in perspective.
- Forcing a long horizontal scroll, city walls or medieval costume on every subject → the source suggests logistics parks, clinics, greenhouses.

## Nearest neighbours

- **51 · Persian miniature:** also dense, readable multi-scene spaces, but stacked flat planes with geometric patterns everywhere; Lorenzetti is solid city buildings and continuous daily life without pattern fill.
- **38 · Giotto:** same fresco surface, but a few large weighty figures in one event; Lorenzetti is a whole society of small figures.

## Source

Adapted from huashu-art-motion · `43_lorenzetti.md` (research recipe; v2 three rounds, self-score ~72; shortcomings: needs more crowd, painted wall texture, natural figure proportions and a receding street; only two distinct reference works).
