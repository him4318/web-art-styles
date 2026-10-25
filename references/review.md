# Reviewing a styled UI

How to decide whether a styled screen is actually "in the style" and still a good product screen, and how to report it honestly.

## Why styled UIs fail

When a batch of research styles was built without this protocol, almost all of them failed the same way:

1. **Nobody looked at the originals.** The work was done from memory, so most results were one generic modern flat illustration in different colours: clean vector shapes, a palette, a layer of noise.
2. **"Don't copy famous works" was over-applied.** The rule is there to stop copying a painting's people and layout, not to stop learning its brushwork, materials, colour relationships and how it draws figures.
3. **No medium layer.** The strongest styles (Van Gogh, ink) run every shape through a real material: flow-field strokes, dry brush, ink bleed. The weak ones only drew shapes.
4. **One subject for every style.** Using the same scene across styles ignored each style's own logic of composition (Ma Yuan's corner, the miniature's stood-up ground).
5. **No bar to clear.** "It renders" counted as done; nobody held it next to the original and asked whether it looks like the artist.

## The protocol

### 1. Anchor on the originals

Gather **at least three different works** (public-domain museum images are ideal) and actually look at them side by side. Write:
- **Five signature traits**, each marked *must have*, *bonus* or *taboo*. For example, Ma Yuan: *must* = one-corner composition, large empty space, "axe-cut" texture strokes; *bonus* = willows, a tiny figure looking into the void; *taboo* = filled edge to edge, bright colour.
- **The main palette**, noting where the lights lean (warm or cool) and where the darks lean.
- **How it's physically made.** That process is your rendering spec.

Check the images you collected. Searches mix in maps, modern photos, and gallery shots with frames. Two "different" works may be one image, colour-shifted.

### 2. Compose by the style's own logic

Use how the style organises a picture (see `translating-a-style.md`), applied to your page layout. Write one sentence: "this style composes by …".

### 3. Clean form, then the medium pass

Build the form layer (layout, components, illustrations) cleanly, then pass it through the medium layer (edges, texture, filters, marks). If a medium recipe is reusable, put it in a shared utility rather than inside one page.

### 4. One screen first, side by side

Make a still of one real screen before adding motion. Put it next to the reference contact sheet. "Recognisable at a glance as that artist?" can only be answered with the two side by side.

### 5. Bring it to life with the style's own motion

One main interaction moment plus two small ambient motifs, each moving the way that material moves:
- ink drifts with the wind;
- brushstrokes breathe;
- water breaks reflections up;
- a fresco barely moves, but gazes and hand-offs do.

Don't give every style the same fade-in.

### 6. Score it: 80 to pass, at most two rounds

| Item | Points | How to judge |
|---|---|---|
| Signature traits | 35 | Tick each *must have* from step 1; minus 7 for each one missing |
| Believable medium | 20 | Zoom to 1:1: real material, or vectors with noise? |
| Composition and clarity | 15 | Uses the style's own way of organising space? Is the screen's purpose clear at a glance? |
| Illustrations and figures | 15 | Do they have weight and relationships? Childish or sticker-like scores below 8 |
| Motion and states | 10 | Alive without flicker; motion tied to real events (loading, done, hover) |
| Product quality | 5 | Contrast, reduced motion, phone width, performance (see the UI checks below) |

**Two automatic fails:** signature traits below 25, or the main illustration or subject reading as childish doodling.

- **Who judges what:** an AI reviewer judges only the factual items. Taste ("is the composition beautiful?") belongs to the person who owns the design. A self-score is for ranking, not for declaring something finished.
- **Self-scores run high.** In the source project they averaged about 10 points (up to 17) above independent review. Always record item scores and the signature checklist, put an independent score next to the self-score, and use the lower one.
- **After two rounds below 80:** keep the status "research recipe" and write what blocked it into the card's pitfalls.

## UI checks (every styled screen)

- [ ] **Contrast:** body text WCAG AA (4.5:1) and large text 3:1, in every theme, including text over textures and glows. Measure the darkest and lightest points under the text, not the average.
- [ ] **States:** hover, focus-visible, active, disabled, loading, empty, error and done all exist and read in the style. Focus rings are never clipped by a decorative clip-path.
- [ ] **Reduced motion:** with `prefers-reduced-motion: reduce`, nothing loops, nothing flickers and decorative canvases show a still frame.
- [ ] **Widths:** works at 375px with no horizontal page scroll, at a laptop width, and at a wide desktop. Decorative layers don't cover content on small screens.
- [ ] **Themes:** light and dark both designed (or a deliberate single theme), and the texture or effects layer doesn't drop out in one of them. One real bug: a `background` shorthand that included a variable set only by JavaScript dropped the whole background whenever the script hadn't run. Set colour and texture in separate properties.
- [ ] **Performance:** no full-screen filter re-rendered per frame; textures drawn once; canvas DPR capped at 2; loops pause when the tab is hidden; layout stays stable while fonts and textures load.
- [ ] **Fonts:** style fonts load before or with the look, and the fallback doesn't break the layout.
- [ ] **Scope:** the look applies only where intended, such as under its data attribute, and switches off cleanly.
- [ ] **Real content:** checked with real data, long names and real error messages, not only the happy-path mock.

## Status labels

Use these words exactly, in cards and in reports:

- **Research recipe:** a sourced hypothesis with no tested implementation.
- **Built ★ / ★★ / ★★★:** implemented and reviewed. ★★★ means recognisable at a glance and usable as is; ★★ means recognisable with clear weaknesses; ★ means needs a redo.
- **Built on one screen** vs **proven across screens:** say which screens, themes and widths were actually checked.

## Source

Adapted from huashu-art-motion · `references/16-风格演示95分协议.md`. The rubric, the automatic fails and the self-score warning are from there; the UI checks are additions from building styled app looks.
