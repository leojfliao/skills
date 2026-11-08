---
name: solution-architecture-diagram
description: Turns a component list, call relationships and deployment environment into a single self-contained SVG system architecture or solution design diagram, with hand-drawn icons for cloud, compute, gateway, database, cache, message queue, storage, security and observability, organised by layer and trust zone with a legend and labelled connectors. Use when the user asks to "draw the architecture diagram for this solution", "produce a system context diagram", "show the cloud, databases and middleware", "make a solution design diagram", or "sketch the deployment topology". Technical architecture diagrams only: no UI mockups, no flowcharts or sequence diagrams, no PPTX, no invented components, no raster output.
version: 1.2.1
agent_created: true
---

# System Architecture & Solution Design Diagram

Produce **one** `.svg` file: vector, scalable, editable, and complete when opened offline. It shows how the system is built, not how the story is told. All labels and copy are in English.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| Architecture diagram, system context, deployment topology, solution design, layered diagram | This Skill |
| Talk outline, speaker script, retrospective story | `internal-share-narrative` |
| A clickable single-file presentation deck | `fintech-presentation` |
| Business process, BPMN, sequence diagram, state machine | Decline; state that it is out of scope |
| UI mockups, web visual design | Decline; state that it is out of scope |

If the user says "draw a diagram" but describes steps, approvals, or time order, confirm once whether they mean architectural relationships or process sequence. Do not guess.

## 2. Input

Ask for all of the following at once, only for what is missing:

1. **Component list**: systems, services, stores, external parties, and their names.
2. **Call relationships**: who calls whom, synchronous or asynchronous, key data-flow directions.
3. **Deployment environment**: public cloud / private cloud / on-premises / hybrid, and whether it is split by region.
4. **Boundaries**: trust zones, network segments, team ownership, what is external.
5. **Diagram type**: context / container / deployment / data flow; if not given, recommend one based on component count.

With an incomplete list, draw only what is known and put unknowns in a "To be confirmed" legend box — **never add a plausible-looking component**. For a sketch or a prose description, restate it as a "component / type / boundary / connection" table for confirmation before drawing.

## 3. Load resources

- Read `@references/visual-system.md` for colour, stroke width, type size, and whitespace. Follow it exactly; invent no new palette.
- Read `@references/diagram-rules.md` for layering, connectors, legends, dimensions, and self-containment.
- Read `@references/icon-catalog.md` for icon semantics; the icon bodies are in `@assets/icon-library.svg`.
- Start from `@assets/diagram-shell.svg`, keeping its grid, legend area, and `<defs>` structure; replace only the components and connectors.

## 4. Diagram types

| Type | Granularity | Ceiling |
|---|---|---|
| System context | The system, external actors, main data-flow directions | 8 boxes |
| Container / layered | Ingress, service, and data tiers with their components | 20 components |
| Deployment topology | Regions, availability zones, instances, network boundaries | 3 regions × 8 nodes |
| Data flow | Path from producer to consumer with intermediate stages | 10 stages |

Above the ceiling, split into two diagrams rather than shrinking type. One diagram answers one question.

## 5. Steps

1. Turn the input into a "component / type / boundary / connection" table and show it to the user first.
2. Copy `diagram-shell.svg`, fix the canvas size and tier bands, and choose vertical tiers or partitioned layout according to the diagram type.
3. Pick the needed icons from `@assets/icon-library.svg` and **copy each complete `<symbol>` block into the output file's `<defs>`**, then reference it with `<use href="#icon-xxx">`. Never write cross-file references.
4. Place components by tier: external at top or left, then ingress, services in the middle, data at the bottom. Request direction is uniformly top-to-bottom or left-to-right; never mix the two in one diagram.
5. Draw connectors: solid for synchronous calls, dashed for asynchronous messages, each with a short verb label. Do not duplicate return paths unless the return is the point of the diagram.
6. Add dashed boundary boxes for trust zones or network segments, with the zone name in the top-right corner.
7. Add a legend covering icon meaning, line-style meaning, and colour meaning. Supply any missing block.
8. Add a title, a one-line caption, and component IDs that match references in the body text.
9. Run the section 9 checklist before delivering.

## 6. Output contract

- **Single deliverable**: one `.svg` file, UTF-8, with `viewBox`, `role="img"`, and `aria-label`.
- **Self-contained**: all `<symbol>` definitions inlined in `<defs>`; no external `href` / `xlink:href`, no `<image>` links, no external fonts, no CSS `@import`.
- **Editable**: components, connectors, and text remain separate grouped elements with readable `id` / `class` names. Never flatten the whole diagram into one path; never rasterise.
- **Scalable**: scale only via `viewBox`; minimum text 10px, uniform stroke width 1.5, still readable after zoom.
- **No external dependencies**: icons render offline and no network resource is requested.
- **Excludes**: real customer names, internal system codenames, real capacity or transaction volumes, and any vendor logo. All samples are labelled "sample". All copy is in English.

## 7. Icon rules

- Use only icons from `@assets/icon-library.svg`, chosen by the semantics in `@references/icon-catalog.md`; never sketch a new off-style icon on the fly.
- When no exact semantic match exists, use the closest generic icon plus an explanatory text label. Do not force a fit.
- Every icon inherits `stroke="currentColor"`, `fill="none"`, `stroke-width="1.5"`. Do not change colour or width for an individual icon; only the focal component may switch its stroke to red.
- Icons are uniformly 24×24 canvas units and align to the text baseline. Never resize them ad hoc.

## 8. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write an SVG file | Output a "component / type / boundary / connection" Markdown table plus a layered text description, marked "not drawn", with recovery steps |
| Component list too thin for a coherent thread | Stop at the confirmation table; do not invent components |
| Component count exceeds the type ceiling | Split the diagram or suggest a different type; never shrink type to fit |
| User asks for a vendor's official icon pack | Explain the redistribution licensing issue; offer hand-drawn icons plus text labels |
| User asks for PNG / raster export | Explain this Skill is vector-only; suggest "open the SVG in a browser and save as" |
| Material appears to contain non-public information | Stop, list the items, and ask for redaction before drawing |
| Real intent is a flowchart / sequence diagram / UI mockup | State the scope per section 1; do not take the job |

## 9. Pre-delivery checklist

- [ ] Opens offline in a browser with every icon visible and zero network requests
- [ ] Every `<use>` `href` resolves to an `id` in this file's `<defs>`
- [ ] Components match the confirmation table one-for-one, with nothing extra or missing
- [ ] Layering direction is consistent; vertical and horizontal are not mixed
- [ ] Every connector has a verb label; solid / dashed semantics match the legend
- [ ] Boundary boxes, legend, title, and caption are present
- [ ] Red marks exactly one focal path; everything else is black / white / grey
- [ ] Text is at least 10px, stroke width uniformly 1.5
- [ ] No real business data, no vendor logos, no external references
- [ ] Checked at 200% and 50% zoom: no blurring, no overlap
- [ ] All labels and captions are in English

Fix anything that fails before delivering; never ship a known issue.

## 10. Handover

Give the file path, diagram type, component count, connector count, one line of legend explanation, and one hint:

"The SVG can be inlined directly into a `fintech-presentation` figure slide."

Do not put production notes, status labels, or template comments inside the SVG.
