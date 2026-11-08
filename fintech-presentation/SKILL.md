---
name: fintech-presentation
description: Turns an approved content outline into a single self-contained interactive HTML presentation deck with restrained enterprise-finance visuals (red / white / black, tabular numerals, fine grid) and professional data displays for FinOps, service metrics, solution comparisons, reference architecture and delivery roadmaps. Includes in-slide controls (period and series switchers, chart tooltips, expandable rows, sortable tables, option tabs), keyboard navigation, section jumps, a progress bar, an Esc overview, speaker notes, and print expansion, with zero external dependencies. Use when the user asks to "make an internal tech talk deck", "turn this outline into clickable slides", "produce a one-page HTML deck in the HSBC style", "build a deck you can click through", "show FinOps cost vs budget as slides", "put our service metrics into a deck", or "lay this script out as slides". Interactive HTML deck only: no PPTX, no scrolling long-read, no batch PDF processing, no rewriting the user's content, no invented data.
version: 1.3.0
agent_created: true
---

# Interactive Enterprise Finance Deck

Produce **one** `.html` file: double-click to open, works offline, fully navigable by keyboard, and rich enough to carry a real engineering review. The user supplies the content; this Skill only places it into a restrained, precise, clickable layout. All visible copy is in English.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| A clickable single-file presentation deck, including FinOps / metrics / options pages | This Skill |
| A scrolling long-read HTML + PDF | `html-longread-story` |
| A `.pptx` file, or slides generated from a batch of conference PDFs | `conference-pdf-to-deck` |
| Layout QA on an existing PPTX | `pptx-layout-qc` |
| Narrative outline, speaker script, retrospective story | `internal-share-narrative` |
| A system architecture or solution design diagram as a standalone SVG | `solution-architecture-diagram` |

If the user says "deck", "slides", or "presentation" but explicitly wants a PPTX file, route it and explain that this Skill outputs HTML only. The same applies to "make this `.pptx` clickable": this Skill does not parse PPTX files. A reference-architecture *slide* belongs here; a standalone architecture *diagram* belongs to `solution-architecture-diagram`.

## 2. Input

Confirm all four before starting; ask once for whatever is missing:

1. **Content**: an approved outline, script, or slide-by-slide content. Markdown, plain text, or given directly in chat.
2. **Occasion and duration**: internal talk / review / reporting, and how many minutes. Sets slide count and information density.
3. **Audience**: peer engineers / cross-functional / leadership. Sets terminology density and annotation level.
4. **Data and assets**: verified figures for any chart, architecture SVGs, option names, or legend notes. If figures are not verified, say so and use a table instead of a chart.

When content is not final, route to `internal-share-narrative` first; do not write and lay out at the same time. Estimate 1.5–2 minutes per slide; prefer fewer slides over cramped text.

## 3. Load resources

- Read `@references/visual-system.md` for colour, type, spacing, chart rules, and motion. Follow it exactly; invent no new palette.
- Read `@references/layout-and-interaction.md` for the layout library, the component snippets, the data contract, and the interaction state machine. Choose layouts by content; do not make every slide look the same.
- Start from `@assets/deck-shell.html`: a working 14-slide example deck whose CSS variables, chart renderers, controls, key map, overview, speaker notes, print expansion, and brand mark already work. **Do not rewrite the shell — replace its sample content and data, then add or remove slides.**
- Read `@assets/brand-mark.svg` for the brand mark: `<symbol id="brand-hex">` is the page's only definition of the mark. Do not draw another one and do not use cross-file references. Usage, official-asset replacement, and the on/off switch are in the "Brand Mark" section of `@references/visual-system.md`.

The shell's example deck covers: cover, agenda, three section dividers, FinOps cost vs budget, savings levers, service trend, SLA posture, option comparison, reference architecture, delivery roadmap, risk register, and closing. Delete the sections you do not need rather than inventing new scaffolding.

## 4. Steps

1. Split content into slides, assign a layout and (where relevant) an interaction type to each, and write a "slide / title / layout / source section" mapping table for the user to review first.
2. Copy `deck-shell.html`, keep the `<style>`, `<script>`, and `<symbol id="brand-hex">` logic intact, and change only the `<section class="slide">` content inside `<body>` plus the sample data in the script.
3. Every `<section>` must have a non-empty `data-title`; the TOC and overview are generated from it. Add `data-chapter` for overview grouping and a `<template class="notes">` for speaker notes.
4. Replace every sample value. The chart series live in the `MONTHS`, `CATS`, `QUARTERS`, `METRICS`, `OPTIONS`, `LAYERS`, and `PHASES` objects in the script; figures must come from the user, never from the sample defaults.
5. Fill content according to the layout table. Wrap numbers in `<span class="num">` for tabular alignment. Keep the existing control `data-*` attributes when reusing a component — the event delegation depends on them.
6. To embed an architecture diagram, inline the SVG source directly; never write `<img src="...">` pointing to a file. Draw charts only with inline SVG or pure CSS, with no chart library.
7. Check red coverage, type hierarchy, and alignment globally, then run the checklist below item by item.
8. Output the file path, slide count, and keyboard instructions.

## 5. Output contract

- **Single deliverable**: one `.html` file, UTF-8, with all CSS, JS, and SVG inlined.
- **Zero external requests**: no resource references starting with `http://`, `https://`, or `//cdn`; no `<link>`, `<script src>`, or `<img src>` pointing outward.
- **Working navigation**: `→` / `Space` / `PgDn` next, `←` / `PgUp` previous, `Home` / `End` first / last, `Esc` toggles overview, `N` speaker notes, `F` fullscreen, `?` shortcut list, TOC / progress-bar / agenda-card clicks jump, and URL `#N` deep links work.
- **Working in-slide controls**: segmented switches redraw their chart and KPI strip, toggles add or remove a single focal highlight, chart points show a tooltip on hover and on focus, expandable rows toggle with `aria-expanded`, table headers sort with `aria-sort`, and option / tier / phase controls update their detail panel.
- **Accessibility**: visible focus on every control, `prefers-reduced-motion` honoured, `role="img"` plus `aria-label` on inline SVGs, and no control reachable only by mouse.
- **Print**: under `@media print`, all slides expand one per page; all chart views are rendered up front and expanded; controls and tooltips are hidden.
- **Excludes**: real customer names, account data, internal system names, real transaction volumes, and unconfirmed third-party brand marks. Sample data must be labelled "sample".
- **Brand mark**: at most one HSBC Hexagon redraw in the top-right corner, following the colour, size, and placement rules in "Brand Mark". When the user explicitly wants no mark, set `<html>`'s `data-brand` to `hide`.
- **Language**: all visible copy, labels, captions, and sample content are in English.

## 6. Content discipline

- Do not rewrite, cut, or add to the user's points and conclusions. If a subject is missing or wording is unclear, ask once; do not polish it into a different meaning.
- Do not invent numbers. Leave the position empty and mark it TBD; never deliver the shell's sample figures as if they were real.
- Do not add slides unilaterally. Ask before adding a "Summary" or "Q&A" slide.
- Present facts, judgements, and recommendations as the user wrote them; never add inferential phrases such as "this shows that" or "the results prove".
- Break long sentences only by line or hierarchy, never by meaning; keep technical terms as-is rather than simplifying them.

## 7. Visual and interaction notes

- Red is for focal points only: progress bar, current TOC item, one key figure, one bar, the latest data point, and the target line. Keep red under 10% of a screen; variance labels and status chips stay neutral unless they are the single focus.
- Build the "Fintech feel" with whitespace, weight, tabular numerals, and a 1px grid — no gradients, glows, glassmorphism, or large radii.
- Angular detailing is limited to structural chamfers and one narrow angled band on section dividers. Never tile or repeat the Hexagon as decoration.
- Transitions are 120–200ms, fade and vertical displacement under 8px only; nothing autoplays or auto-advances.
- Per-slide ceiling: one main point plus no more than 5 supporting items. Split the slide rather than shrinking type.
- Replace every piece of shell sample content; do not leave the word "sample" in the deliverable unless it genuinely labels sample data.
- Keep the brand mark fixed in the top-right corner: no animation, no recolouring, never enlarged enough to compete with the title. A white badge holds it on dark covers.
- Controls never fight paging: elements carrying `data-no-paging` swallow arrow keys and Space so a focused control does not flip the slide.

## 8. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write an HTML file, or no rendering environment | Output a structured Markdown slide outline (slide / title / layout / content), marked "not rendered", with recovery steps |
| Content appears to contain non-public data | Stop, list the specific items, and ask for redaction before continuing; do not write the original text into the output |
| Verified chart figures are missing | Use a data table, or omit the chart. Never ship the shell's sample numbers |
| User asks for a hosted chart library, CDN fonts, or online images | Decline, explain that zero external dependencies is a hard constraint, and offer an inline alternative |
| User asks for autoplay, background music, or cursor effects | Decline, explain the conflict with the restraint principle, and offer a static alternative |
| Not enough content for one slide but multiple slides requested | Stop and suggest more content or a single-page layout |
| Real intent is PPTX / long-read / architecture diagram | Route per section 1; do not take the job |

## 9. Pre-delivery checklist

- [ ] Opens offline with zero requests in the network panel
- [ ] Every `<section>` has a non-empty `data-title` and, where useful, `data-chapter` plus speaker notes
- [ ] Keyboard reaches every slide and every control; `Esc` overview, `N` notes, `F` fullscreen, `?` help all work
- [ ] TOC item count matches slide count; progress bar reaches 100% on the last slide
- [ ] Every segmented control, toggle, expandable row, sortable header, and detail panel was exercised
- [ ] Chart tooltips appear on hover and on keyboard focus; all numbers use tabular alignment
- [ ] All shell sample data replaced with verified figures, or the chart removed
- [ ] Red coverage under 10%; no gradients, glows, or large radii; angular detailing used sparingly
- [ ] No transitions under `prefers-reduced-motion`
- [ ] Print preview is not clipped, loses no pages, expands every chart view, and has no leftover opacity
- [ ] Shell sample content cleared, no real business data; brand mark undistorted and within limits, switched off if the user asked
- [ ] Mark is either the redraw or an official replacement; no cross-file SVG references
- [ ] Content traces back line by line with no rewriting or new conclusions
- [ ] All visible copy is in English

Fix anything that fails before delivering; never ship a known issue.

## 10. Handover

Give the file path, total slide count, and one line per slide describing its layout and interaction, then one line of operating instructions:

`← → to navigate · Esc for overview · N notes · F fullscreen · ? shortcuts · on-slide controls are clickable and keyboard reachable · print directly to export PDF`

No extra documentation, and no production notes or status labels inside the HTML.
