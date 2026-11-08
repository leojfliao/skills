---
name: fintech-presentation
description: Turns an approved content outline into a single self-contained interactive HTML presentation deck with restrained enterprise-finance visuals (red / white / black, tabular numerals, fine grid), keyboard navigation, section jumps, a progress bar, an Esc overview, and print expansion, with zero external dependencies. Use when the user asks to "make an internal tech talk deck", "turn this outline into clickable slides", "produce a one-page HTML deck in the HSBC style", "build a deck you can click through", or "lay this speaker script out as slides". Interactive HTML deck only: no PPTX, no scrolling long-read, no batch PDF processing, no rewriting the user's content, no invented data.
version: 1.2.0
agent_created: true
---

# Interactive Enterprise Finance Deck

Produce **one** `.html` file: double-click to open, works offline, fully navigable by keyboard. The user supplies the content; this Skill only places it into a restrained, precise, clickable layout. All visible copy is in English.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| A clickable single-file presentation deck | This Skill |
| A scrolling long-read HTML + PDF | `html-longread-story` |
| A `.pptx` file, or slides generated from a batch of conference PDFs | `conference-pdf-to-deck` |
| Layout QA on an existing PPTX | `pptx-layout-qc` |
| Narrative outline, speaker script, retrospective story | `internal-share-narrative` |
| A system architecture or solution design diagram | `solution-architecture-diagram` |

If the user says "deck", "slides", or "presentation" but explicitly wants a PPTX file, route it and explain that this Skill outputs HTML only. The same applies to "make this `.pptx` clickable": this Skill does not parse PPTX files.

## 2. Input

Confirm all four before starting; ask once for whatever is missing:

1. **Content**: an approved outline, script, or slide-by-slide content. Markdown, plain text, or given directly in chat.
2. **Occasion and duration**: internal talk / review / reporting, and how many minutes. Sets slide count and information density.
3. **Audience**: peer engineers / cross-functional / leadership. Sets terminology density and annotation level.
4. **Existing assets**: architecture SVGs, chart data, or legend notes that need embedding.

When content is not final, route to `internal-share-narrative` first; do not write and lay out at the same time. Estimate 1.5–2 minutes per slide; prefer fewer slides over cramped text.

## 3. Load resources

- Read `@references/visual-system.md` for colour, type, spacing, and motion. Follow it exactly; invent no new palette.
- Read `@references/layout-and-interaction.md` for layouts and interaction. Choose layouts by content; do not make every slide look the same.
- Start from `@assets/deck-shell.html`: its CSS variables, state machine, key map, overview, and print styles already work. **Do not rewrite the shell — replace its sample content and add or remove slides.**
- Read `@assets/brand-mark.svg` for the brand mark: `<symbol id="brand-hex">` is the page's only definition of the mark. Do not draw another one and do not use cross-file references. Usage, official-asset replacement, and the on/off switch are in the "Brand Mark" section of `@references/visual-system.md`.

## 4. Steps

1. Split content into slides, assign a layout type to each (see the layout table), and write a "slide / title / layout / source section" mapping table for the user to review first.
2. Copy `deck-shell.html`, keep the `<style>`, `<script>`, and `<symbol id="brand-hex">` logic intact, and change only the `<section class="slide">` content inside `<body>`.
3. Every `<section>` must have a `data-title`; the TOC and overview are generated from it. Never leave a title empty.
4. Fill content according to the layout table. Wrap numbers in `<span class="num">` for tabular alignment.
5. To embed an architecture diagram, inline the SVG source directly; never write `<img src="...">` pointing to a file.
6. Draw charts only with inline SVG or pure CSS, with no chart library. With no confirmed data, use a data table instead of a chart.
7. Check red coverage, type hierarchy, and alignment globally, then run the checklist below item by item.
8. Output the file path, slide count, and keyboard instructions.

## 5. Output contract

- **Single deliverable**: one `.html` file, UTF-8, with all CSS and JS inlined.
- **Zero external requests**: no resource references starting with `http://`, `https://`, or `//cdn`; no `<link>`, `<script src>`, or `<img src>` pointing outward.
- **Working interactions**: `→` / `Space` / `PgDn` next, `←` / `PgUp` previous, `Home` / `End` first / last, `Esc` toggles overview, TOC click jumps, progress bar click jumps.
- **Accessibility**: visible focus, `prefers-reduced-motion` honoured, correct semantic tags.
- **Print**: under `@media print`, all slides expand, one slide per page, progress bar and TOC hidden.
- **Excludes**: real customer names, account data, internal system names, real transaction volumes, and unconfirmed third-party brand marks. Sample data must be labelled "sample".
- **Brand mark**: at most one HSBC Hexagon redraw in the top-right corner, following the colour, size, and placement rules in "Brand Mark". When the user explicitly wants no mark, set `<html>`'s `data-brand` to `hide`.
- **Language**: all visible copy, labels, captions, and sample content are in English.

## 6. Content discipline

- Do not rewrite, cut, or add to the user's points and conclusions. If a subject is missing or wording is unclear, ask once; do not polish it into a different meaning.
- Do not invent numbers. Leave the position empty and mark it TBD; never fill a chart with placeholder values.
- Do not add slides unilaterally. Ask before adding a "Summary" or "Q&A" slide.
- Present facts, judgements, and recommendations as the user wrote them; never add inferential phrases such as "this shows that" or "the results prove".
- Break long sentences only by line or hierarchy, never by meaning; keep technical terms as-is rather than simplifying them.

## 7. Visual and interaction notes

- Red is for focal points only: progress bar, current TOC item, key figures, one focal data series. Keep red under 10% of a screen.
- Build the "Fintech feel" with whitespace, weight, tabular numerals, and a 1px grid — no gradients, glows, glassmorphism, or large radii.
- Transitions are 120–200ms, fade and vertical displacement under 8px only; nothing autoplays or auto-advances.
- Per-slide ceiling: one main point plus no more than 5 supporting items. Split the slide rather than shrinking type.
- Replace every piece of shell sample content; do not leave the word "sample" in the deliverable unless it genuinely labels sample data.
- Keep the brand mark fixed in the top-right corner: no animation, no recolouring, never enlarged enough to compete with the title. A white badge holds it on dark covers.

## 8. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write an HTML file, or no rendering environment | Output a structured Markdown slide outline (slide / title / layout / content), marked "not rendered", with recovery steps |
| Content appears to contain non-public data | Stop, list the specific items, and ask for redaction before continuing; do not write the original text into the output |
| User asks for a hosted chart library, CDN fonts, or online images | Decline, explain that zero external dependencies is a hard constraint, and offer an inline alternative |
| User asks for autoplay, background music, or cursor effects | Decline, explain the conflict with the restraint principle, and offer a static alternative |
| Not enough content for one slide but multiple slides requested | Stop and suggest more content or a single-page layout |
| Real intent is PPTX / long-read / architecture diagram | Route per section 1; do not take the job |

## 9. Pre-delivery checklist

- [ ] Opens offline with zero requests in the network panel
- [ ] Every `<section>` has a non-empty `data-title`
- [ ] Keyboard reaches every slide; `Esc` overview opens and closes; click jumps work
- [ ] TOC item count matches slide count; progress bar reaches 100% on the last slide
- [ ] Numbers use tabular alignment; red coverage under 10%
- [ ] No transitions under `prefers-reduced-motion`
- [ ] Print preview is not clipped, loses no pages, and has no leftover opacity
- [ ] Shell sample content cleared, no real business data; brand mark undistorted and within limits, switched off if the user asked
- [ ] Mark is either the redraw or an official replacement; no cross-file SVG references
- [ ] Content traces back line by line with no rewriting or new conclusions
- [ ] All visible copy is in English

Fix anything that fails before delivering; never ship a known issue.

## 10. Handover

Give the file path, total slide count, and one line per slide describing its layout, then one line of operating instructions:

`← → to navigate · Esc for overview · TOC is clickable · print directly to export PDF`

No extra documentation, and no production notes or status labels inside the HTML.
