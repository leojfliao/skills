# Architecture Diagram Drawing Rules

This file locks down the rules for the canvas, layered layout, connectors, legend, and self-contained behavior. The goal is for diagrams drawn by different people to look like the work of one person, and for no icons to be missing when opened offline.

## 1. Canvas and Grid

| Item | Value |
|---|---|
| Base canvas | `viewBox="0 0 1200 720"`, 16:10 |
| Outer margin | 40 |
| Grid | 8px base, component box edges snap to multiples of 8 |
| Component box | Width 180, height 72, corner radius 2, stroke 1.5 |
| Component spacing | Horizontal 32, vertical 40 |
| Tier band spacing | 64 |
| Icon | 24x24, placed on the left inside the box, 16 from the left edge |
| Text | Component name 14px, type label 11px, connector label 11px, title 22px, caption 12px |
| Minimum type size | 10px, unreadable after scaling below this value |

When there are more than 6 components, switch to `viewBox="0 0 1600 900"` instead of shrinking the type size to cram them into the 1200 canvas.

## 2. Layered Layout

Use a uniform **top-to-bottom** order: external participants -> access tier -> service tier -> data tier. A data flow diagram may switch to **left-to-right**, but a single diagram may pick only one direction, never mix them.

```text
┌───────────────────────────── Title Area ─────────────────────────────┐
│ [External]  Users / Partners / External Systems                      │
├──────────────────────────────────────────────────────────────────────┤
│ [Ingress]  Gateway / Load Balancer / API / Firewall                  │
├──────────────────────────────────────────────────────────────────────┤
│ [Service]  Service A / Service B / Scheduler / Proxy                 │
├──────────────────────────────────────────────────────────────────────┤
│ [Data]  Database / Cache / Message Queue / Object Storage            │
├──────────────────────────────────────────────────────────────────────┤
│ Legend Area                                                          │
└──────────────────────────────────────────────────────────────────────┘
```

- Write the tier name on the left of each tier (11px, secondary gray), separate tiers with a 1px divider line, and never draw solid color blocks.
- Within a tier, arrange components left to right in call order, not randomly by importance.
- Cross-tier connectors use orthogonal routing (horizontal + vertical), never diagonal lines; when two lines overlap, offset them by 8px.
- Component IDs `C01`, `C02` go in the top-right corner of the box and match the references in the text.

## 3. Connectors

| Type | Style | Use |
|---|---|---|
| Synchronous call | Solid 1.5px `#333333`, solid arrowhead | A single call with a clear request / response |
| Asynchronous message | Dashed `stroke-dasharray="6 4"`, solid arrowhead | Queues, events, callbacks |
| Data flow | Solid 2px `#767676`, hollow arrowhead | When data movement is the main subject |
| Boundary / grouping | Dashed 1px `#D7D8D6`, no arrowhead | Trust zones, network segments, enclosing components |
| Focal path | Solid 2px `#DB0011` | At most one per diagram, marking the chain to be discussed |

- Every connector carries a short verb phrase label ("read", "write", "publish", "authenticate"), placed on a small white rectangle so the line does not cut through it.
- Do not draw the return path twice, unless the return itself is the point being made.
- Define all arrowheads uniformly with `<marker>` in `<defs>` and do not hand-draw triangles on each line.
- Connectors must not pass through component boxes or tier-name text.

## 4. Boundary Boxes and Legend

- Trust zones, network segments, and team ownership use dashed boxes, with the zone name in the top-right corner and no fill inside.
- The legend always has three blocks: **icon meaning** (list each icon used once), **line-style meaning** (solid / dashed / focal line), **color meaning** (black-gray / red). Missing any one counts as incomplete.
- Place the legend in a separate area at the bottom of the canvas, separated by a 1px top border, never overlapping components.
- There must be a caption line under the title stating "what question this diagram answers" and a "sample / real" mark.

## 5. Self-Contained (the rule most likely to break)

Browsers often block cross-file SVG references under `file://`. Therefore:

- **Do not** write cross-file references such as `<use href="icon-library.svg#icon-database">`.
- **Do** copy the full `<symbol>` for each used icon from `assets/icon-library.svg` into the `<defs>` of the output file, then reference it with `<use href="#icon-database">` (a same-file reference).
- Copy only what is used, do not stuff all 47 in; but what you copy must be the complete `<symbol>`, with the path intact.
- Inline `<marker>`, gradients (gradients are forbidden by this spec), and all styles; do not write `<style src>` or `@import`.
- Before delivery, check one by one: after removing the `#` from the `href` of every `<use>`, a matching `id` can be found in the `<defs>` of this file.

## 6. Accessibility and Editability

- The root `<svg>` carries `role="img"` and `aria-label`, describing the whole diagram in one sentence.
- Each component group carries a `<title>`, use `<text>` for text rather than converting glyphs to paths, so names can be edited later.
- Put components, connectors, boundaries, and legend into named `<g id="components">` / `<g id="flows">` / `<g id="boundaries">` / `<g id="legend">` respectively.
- Do not rasterize, do not merge the whole diagram into a single `<path>`, and do not embed bitmaps with `<image>`.

## 7. Common Pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| Cross-file `<use>` | All icons blank when opened locally | Inline the symbol into `<defs>` |
| Mixed icon styles | Some stroke widths thick, some with fills | Use only symbols from the icon set, inheriting a uniform stroke |
| Diagonal lines crossing | Connectors cross and cover text | Orthogonal routing, offset 8px at overlaps |
| Red overuse | Three red chains, the focal point disappears | Keep only one focal path per diagram |
| Text too small | Labels blur together after scaling to 50% | Minimum 10px, split into multiple diagrams if over the limit |
| Legend missing line styles | Dashed lines make no sense | All three legend blocks present |
| Invented components | Systems appear that the user never mentioned | Put unknown parts in a "to be confirmed" box, do not add them |
| Too many components | 20 boxes crammed into one tier | Split into multiple diagrams or switch diagram type |
| Mixed directions | Some top-bottom, some left-right | One uniform direction for the whole diagram |
| Lines without labels | Impossible to tell what a call does | Add a verb phrase to every line |
| Real info in the diagram | Internal system names leak when shared publicly | Use abstract names, and add a separate note on what to redact |

## 8. Acceptance

- [ ] Opens offline, every `<use>` has a matching `id`, zero network requests
- [ ] Components match the confirmation list item by item, nothing extra or missing
- [ ] Layered direction is uniform, all connectors are orthogonal routing and labeled
- [ ] Boundary boxes, three legend blocks, title, and caption all present
- [ ] Red, black, gray, white taken from the visual spec, red marks only one focal point
- [ ] Text no smaller than 10px, stroke width uniform at 1.5
- [ ] Groups clearly named, `<text>` editable
- [ ] Checked once at both 50% and 200% zoom, no overlap, no blurry text
- [ ] No real business data, no vendor logo, no external references
