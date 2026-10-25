# Translating an art style into a UI

Read this when you add a style, use a known style for a new kind of product, need to separate two close styles, or are given only an artist's or studio's name.

## Four things to keep apart

- **Style language:** materials and mark-making, colour relationships, light, how forms are drawn, how space is organised. This is what should stay recognisable after the subject changes.
- **Composition tendency:** for example weight pushed into one corner, figures linked by their gaze, big jumps in scale. Re-solve it for your screen sizes and content; it is not a fixed set of coordinates.
- **Content and motifs:** the people, clothes, bridges, boats, gardens and gods in famous works. They do not come along with the style unless the brief needs them.
- **Purpose and behaviour:** what the screen is for, what the user does, what changes when they act. Speed and motion follow the product, not the painting. A quiet style can still have a fast, crisp interaction; a limited-animation style can still show complex state.

Choosing an artist is not choosing a famous painting as a template. For a studio (Ghibli, Pixar), first narrow the scope to particular films, directors or periods. A studio is not one filter.

## Study across works

When you set up a style for the first time, look at **at least three different works**: different subjects, scales or lighting. Consecutive frames, crops of one painting, or one composition in different colours count as one sample. If material is missing, say so; don't pad the list or claim to have seen what you haven't.

Then write a short **style brief** next to the design:

1. **Scope and sources:** which works, by whom, in what medium and period. Check the attribution.
2. **Stable traits:** 3–5 observable traits, each noting which references support it and which don't.
3. **Neighbours:** two concrete differences from the nearest neighbouring style. For example, Renoir, Pissarro and Sisley cannot share one broken-brush filter and call it done.
4. **Your UI:** the product, its screens, its users, its content and its devices, rebuilt for this brief and not borrowed from the references.
5. **Keep, change, test:** the visual relationships to keep, the content you'll change, and the technical route you still need to prove.

Record *what is in the picture* separately from *how the artist made it*. A palette describes colour relationships and light-and-dark organisation. Don't claim a set of hex codes is the artist's official palette, and 3–6 main colours doesn't mean no other pixel may appear: texture, mixing, shadow and reflections follow the main relationships.

## Think about the real medium

Ask how this kind of image is made in the physical world: brushed, carved, printed, glued, inlaid, sprayed, lit, photographed. That physical process is your rendering spec:

| Medium | Becomes, on the web |
|---|---|
| Scissors and paper | Clip-path polygons, pinned drop shadows |
| Ink on absorbent paper | Displacement and blur filters, washes that only ever darken the paper |
| Halftone printing | Radial-gradient dot patterns, misregistered colour plates |
| Mosaic | Tessellated SVG tiles laid along contours |
| Gold leaf | A brushed texture with embossed highlight lines |
| Pixel hardware | Small bitmaps scaled up with nearest-neighbour, stepped timing |
| Fresco | Pigment soaked into plaster: a grain texture under flat colour |

Build in two layers:
- **The form layer:** clean layout, components and illustrations.
- **The medium layer:** the texture, edges, filters and marks that make it look made with that material.

Noise sprinkled over flat vector shapes is not a style.

## Composition follows the style's own logic

Don't copy a painting's layout. Learn how it *organises* a picture:
- **Ma Yuan:** one corner plus empty space.
- **Persian miniatures:** stand the ground up and fill it; cut buildings open.
- **Gauguin:** crop the foreground; divide with diagonals.
- **Giotto:** lock figures together by their gaze.
- **Impressionists:** sky takes half the frame; water drags the shore into broken strokes.

Write one sentence, "this style composes by …", and apply it to page layout: where the weight sits, how much is empty, what frames what.

## Every part goes through the style

Illustrations, mascots, icons, charts and photos must go through the same material treatment as the backgrounds. Otherwise they look pasted on. This was the most common weakness across the source catalogue: backgrounds looked right, characters looked like flat vector stickers.

- **Illustrations:** give them the style's edge (cut, brushed, pixel), its texture and its light direction.
- **Photos:** treat them with the style's medium where it makes sense (halftone, duotone, grain, a paper mount) or frame them like the style would (a gold frame, a cut-paper mount, a pixel border).
- **Text:** the style's material can shape headings and labels. Body text stays plain and readable.
- **Ageing effects:** peeling, stains and scratches stay on backgrounds and away from people and text. A peeling patch on a figure reads as a hole in their clothes.

## Prove it on a new screen first

Show the style on the real product's screens before calling it reusable, and still offer three directions. Stress-test a new treatment on screens that differ:
- a dense one, such as a table or settings;
- an empty one, such as an empty state or onboarding;
- one with a big change of scale, such as a hero next to a small modal.

A landscape painter's style may not work for a dense dashboard. Don't promise every screen will suit every style.

Check stills first:
- Remove the artist's name and any famous motifs. Is the screen still recognisable from its form, colour, light and marks?
- Next to the nearest neighbouring style, is there a real difference?

Then check motion and interaction:
- Do hover, focus, loading and done states keep the style without hurting usability?
- Does it hold up at normal speed and on a phone?

A good still doesn't prove smooth motion. A sensible written plan doesn't prove the screen works.

## Status, honestly

- **Research recipe:** has sources and a clear visual hypothesis, so it can guide an attempt. It has no tested implementation.
- **Built on one screen:** implemented, rendered and checked, but only for that screen. Record the parameters and where it broke.
- **Proven across screens:** different new screens passed an independent visual review, with the covered range recorded. Note interaction checks separately.

One branch of a studio passing doesn't mean the whole family passes. A new card doesn't borrow an old card's rating, performance or acceptance.

## Source

Adapted from huashu-art-motion · `references/15-跨场景风格提炼.md`, with parts of `08-风格作者规范.md` and `16-风格演示95分协议.md`, rewritten for websites and app UIs.
