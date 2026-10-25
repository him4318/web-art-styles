---
name: web-art-styles
description: "Design websites and app UIs in recognisable art styles: 53 style cards (Matisse cut-outs, Chinese ink, Shinkai light, Ghibli watercolour, cave painting, 8-bit, Bauhaus, pop halftone, Klimt gold, Seurat, Haring, Van Gogh, Hopper, vaporwave and more), each translated into palette, type, textures, edges, components, motion and CSS/SVG/canvas techniques, with rendered screen mockups. Use when someone wants a site, page, theme, landing page, dashboard skin or easter-egg look 'in the style of' an artist, art movement, studio or retro medium, asks for a distinctive or handmade visual direction, or wants to turn an art style into design tokens and UI components."
---

# Web art styles

Turn art styles into websites and app interfaces that people recognise at a glance and can still use: readable text, clear hierarchy, working states, fast pages.

The catalogue has **53 styles**: 36 built treatments rated ★–★★★ and 17 research recipes. Each card is in `references/styles/` and listed in [`references/styles/INDEX.md`](references/styles/INDEX.md). Rendered screen mockups are in `assets/mockups/`, with an overview at `assets/mockups/overview.png`.

## When a request comes in

1. **Pin down the brief.** What is being styled (a whole app, one landing page, a single hero, an optional "look" or easter egg)? Who uses it, and how often? Light, dark or both? A daily-use product UI tolerates far less texture and motion than a campaign page.
2. **Pick candidates from the index.** Prefer **Material layer** styles for real products: they work on any subject. Use **Period specimens** for tribute pages, campaigns and easter eggs. Check each card's *Web fit* line.
3. **Offer three directions, not one.** Each direction needs a different *composition idea* (where weight sits, how surfaces are built, what moves), not the same layout recoloured. Show them side by side as the same real screen, with the same content in each, before building anything. A single self-contained HTML page with one panel per direction works well; the mockups in `assets/mockups/` are a ready starting point.
4. **Extract the style's language** before styling anything. Read [`references/translating-a-style.md`](references/translating-a-style.md): separate the *language* (materials, marks, colour relationships, light, form, space) from the *content* of famous works (their people, objects, layouts). Carry over the language and rebuild the content for the new screen.
5. **Build in layers.** Tokens (palette, type) → surfaces, texture and edges → components → motion and "moments" (loading, done, empty) → effects layer. Techniques are in [`references/web-techniques.md`](references/web-techniques.md); palettes and colour rules are in [`references/palettes.md`](references/palettes.md).
6. **Review against the style and the product.** Use [`references/review.md`](references/review.md): compare side by side with reference artworks, run the signature-trait checklist, then the UI checks (contrast, states, reduced motion, phone width, performance). Report the status honestly.

## Rules that always apply

- **Readable first.** Body text meets WCAG AA in every theme. Style fonts go on headings, labels and short UI text; long text stays in a readable face and size.
- **Respect `prefers-reduced-motion`.** Every animated treatment needs a still fallback.
- **No default "AI" palettes.** Avoid blue-to-purple gradient grounds, purple starfields, neon cyan-and-purple glows and purple frosted glass unless the style itself demands them (vaporwave, *The Starry Night*), and say why when you use them. See `palettes.md`.
- **Texture on surfaces, not on reading.** Grain, cracks, stains and peeling stay on backgrounds and decorative areas, never over body text or a person's face.
- **Draw once, move cheaply.** Expensive textures (lit rock, gold leaf, watercolour washes) are rendered once to an image or tile and reused. Animate with `transform` and `opacity`. Cap canvas DPR at 2 and pause loops when the tab is hidden.
- **Scope the style.** Apply a look under one attribute (for example `:root[data-look="paper"] [data-app]`) so it can be switched off and never leaks into other parts of the site.
- **Never copy characters or protected artwork.** Studio and cartoon styles (Ghibli, Pixar, Rick and Morty, South Park, Kirby-era comics, rubber-hose) are references for a *visual approach*. Don't reproduce their characters, logos or frames. The same goes for famous paintings: borrow how they're made, not what's in them.
- **Be honest about status.** Research recipes are hypotheses, not proven treatments. Say "research recipe" and don't borrow a built card's rating for them. Don't claim a look works in a theme, page or width you haven't checked.

## Files

| Path | What it's for |
|---|---|
| `references/styles/INDEX.md` | All 53 styles: layer, status, web fit, mockup |
| `references/styles/<id>.md` | One card per style: traits, palette, type, surfaces, layout, components, motion, techniques, performance and accessibility, pitfalls, neighbours |
| `references/styles/_TEMPLATE.md` | The card format, for adding new styles |
| `references/translating-a-style.md` | How to carry a style into a new UI without copying artworks |
| `references/web-techniques.md` | CSS/SVG/canvas recipes per medium, the look-layer architecture, performance and accessibility patterns |
| `references/palettes.md` | Eight ready UI palettes, colour rules and how to make your own |
| `references/review.md` | Reference-anchored review, scoring and honest status labels |
| `assets/mockups/` | Self-contained HTML mockups with rendered PNGs and an overview contact sheet |

## Adding a style

Copy `references/styles/_TEMPLATE.md`. Study at least three different works first (see `translating-a-style.md`), build one real screen, review it with `review.md`, then add the card and an INDEX row with an honest status.
