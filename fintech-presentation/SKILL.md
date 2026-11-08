---
name: fintech-presentation
description: Turns an approved outline into one self-contained interactive HTML deck with red / white / black enterprise-finance visuals, tabular numerals and zero external requests. Two modes — operations (FinOps, service metrics, SLA tables, option matrices, reference architecture, roadmaps) and commercial pitch, using open frameworks (Sequoia, YC, Presentable business case, 10-slide pitch, Kawasaki 10/20/30) for investor decks, board business cases, TAM / SAM / SOM, traction, unit economics, 2x2 competition, waterfall ROI, tornado sensitivity and the ask. Includes segmented switches, chart tooltips, expandable rows, sortable tables, selectable panels, keyboard navigation, overview, speaker notes, #N deep links and print expansion. Use when asked to "build a commercial pitch deck", "make an investor deck", "write a business case for the board", "show TAM SAM SOM", or "lay this script out as slides". Interactive HTML only — no PPTX, long-read, batch PDF, rewritten content, invented data, or third-party assets.
version: 1.4.0
agent_created: true
---

# Interactive Enterprise Finance Deck

Produce **one** `.html` file: double-click to open, works offline, fully navigable by keyboard, and rich enough to carry a real engineering review or an approval decision. The user supplies the content; this Skill only places it into a restrained, precise, clickable layout. All visible copy is in English.

## 1. Decide whether this Skill should take the job

| What the user actually wants | Route to |
|---|---|
| A clickable single-file presentation deck, including FinOps / metrics / options pages | This Skill · operations mode |
| A commercial / investor pitch, a board business case, TAM / SAM / SOM, traction, "the ask" | This Skill · pitch mode |
| A scrolling long-read HTML + PDF | `html-longread-story` |
| A `.pptx` file, or slides generated from a batch of conference PDFs | `conference-pdf-to-deck` |
| Layout QA on an existing PPTX | `pptx-layout-qc` |
| Narrative outline, speaker script, retrospective story, pitch narrative | `internal-share-narrative` |
| A system architecture or solution design diagram as a standalone SVG | `solution-architecture-diagram` |

If the user says "deck", "slides", or "presentation" but explicitly wants a PPTX file, route it and explain that this Skill outputs HTML only. The same applies to "make this `.pptx` clickable": this Skill does not parse PPTX files. A reference-architecture *slide* belongs here; a standalone architecture *diagram* belongs to `solution-architecture-diagram`.

Pitch mode changes only the **story order and layout choices**; the visual system, navigation, print and accessibility contracts are identical in both modes. A pitch is still an internal-style enterprise deck, not a consumer startup template: no new fonts, no third-party colours, no borrowed artwork.

## 2. Input

Confirm all four before starting; ask once for whatever is missing:

1. **Content**: an approved outline, script, or slide-by-slide content. Markdown, plain text, or given directly in chat.
2. **Occasion and duration**: internal talk / review / reporting / investor pitch / approval committee, and how many minutes. Sets slide count and information density.
3. **Audience**: peer engineers / cross-functional / leadership / investors / board. Sets terminology density and annotation level.
4. **Data and assets**: verified figures for any chart, architecture SVGs, option names, or legend notes. If figures are not verified, say so and use a table instead of a chart.

For **pitch mode**, also confirm:

5. **The decision being asked for**: capital, sponsorship, approval to proceed, or a budget release. This decides which framework fits.
6. **Framework**: your choice from `@references/pitch-frameworks.md`, or "keep my outline order". If the user has not chosen, propose one based on section 7 of that reference and confirm it in one question.

When content is not final, route to `internal-share-narrative` first; do not write and lay out at the same time. Estimate 1.5–2 minutes per slide; prefer fewer slides over cramped text.

## 3. Load resources

- Read `@references/visual-system.md` for colour, type, spacing, chart rules, and motion. Follow it exactly; invent no new palette.
- Read `@references/layout-and-interaction.md` for the layout library, the component snippets, the data contract, and the interaction state machine. Choose layouts by content; do not make every slide look the same.
- **Operations mode**: start from `@assets/deck-shell.html`, a working 14-slide example deck whose CSS variables, chart renderers, controls, key map, overview, speaker notes, print expansion, and brand mark already work. **Do not rewrite the shell — replace its sample content and data, then add or remove slides.**
- **Pitch mode**: read `@references/pitch-frameworks.md` for the framework order, source register, layout mapping, and content discipline, then start from `@assets/pitch-deck-shell.html`, a working pitch example with the same navigation machinery plus the pitch layout library and its renderers. **Do not rewrite the shell.**
- Read `@assets/brand-mark.svg` for the brand mark: `<symbol id="brand-hex">` is the page's only definition of the mark. Do not draw another one and do not use cross-file references. Usage, official-asset replacement, and the on/off switch are in the "Brand Mark" section of `@references/visual-system.md`.
- `samples/` in the repository root contains three finished pitch decks, one per framework family. Read one as a worked example of a filled-in shell; never copy its sample figures into a deliverable.

The operations shell covers: cover, agenda, three section dividers, FinOps cost vs budget, savings levers, service trend, SLA posture, option comparison, reference architecture, delivery roadmap, risk register, and closing. The pitch shell covers: thesis, problem / solution, why now, TAM / SAM / SOM, traction, unit economics, competition, business model, waterfall, tornado, team, and the ask. Delete the sections you do not need rather than inventing new scaffolding.

## 4. Steps

1. **Choose the mode and, in pitch mode, choose the framework** as described in section 2. State the chosen framework before drafting anything. If the user supplies their own outline order, keep it and say so.
2. Split content into slides, assign a layout and (where relevant) an interaction type to each, and write a "slide / title / layout / interaction / source section" mapping table for the user to review first. In pitch mode, add a "framework step" column showing which framework item each slide answers.
3. Copy the matching shell, keep the `<style>`, `<script>`, and `<symbol id="brand-hex">` logic intact, and change only the `<section class="slide">` content inside `<body>` plus the sample data in the script.
4. Every `<section>` must have a non-empty `data-title`; the TOC and overview are generated from it. Add `data-chapter` for overview grouping and a `<template class="notes">` for speaker notes.
5. **Operations mode**: replace every sample value in the `MONTHS`, `CATS`, `QUARTERS`, `METRICS`, `OPTIONS`, `LAYERS`, and `PHASES` objects. **Pitch mode**: replace every sample value in `MARKET`, `TRACTION`, `UNITS`, `RIVALS`, `PRICING`, `SCENARIOS`, `DRIVERS`, `ASK`, `PAINS`, and `SCR`. Figures must come from the user, never from the sample defaults.
6. Fill content according to the layout table. Wrap numbers in `<span class="num">` for tabular alignment. Keep the existing control `data-*` attributes when reusing a component — the event delegation depends on them.
7. To embed an architecture diagram, inline the SVG source directly; never write `<img src="...">` pointing to a file. Draw charts only with inline SVG or pure CSS, with no chart library.
8. In pitch mode, write an **action title** on every slide: a full sentence that carries the conclusion, not a topic label such as "Market". Reading only the headlines must still tell the story.
9. Check red coverage, type hierarchy, and alignment globally, then run the checklist below item by item. In pitch mode also run the framework's acceptance list from `@references/pitch-frameworks.md`.
10. Output the file path, slide count, framework or mode used, and keyboard instructions.

## 5. Output contract

- **Single deliverable**: one `.html` file, UTF-8, with all CSS, JS, and SVG inlined.
- **Zero external requests**: no resource references starting with `http://`, `https://`, or `//cdn`; no `<link>`, `<script src>`, or `<img src>` pointing outward.
- **Working navigation**: `→` / `Space` / `PgDn` next, `←` / `PgUp` previous, `Home` / `End` first / last, `Esc` toggles overview, `N` speaker notes, `F` fullscreen, `?` shortcut list, TOC / progress-bar / agenda-card clicks jump, and URL `#N` deep links work.
- **Working in-slide controls**: segmented switches redraw their chart and KPI strip, toggles add or remove a single focal highlight, chart points show a tooltip on hover and on focus, expandable rows toggle with `aria-expanded`, table headers sort with `aria-sort`, and option / tier / phase / quadrant controls update their detail panel.
- **Accessibility**: visible focus on every control, `prefers-reduced-motion` honoured, `role="img"` plus `aria-label` on inline SVGs, and no control reachable only by mouse.
- **Print**: under `@media print`, all slides expand one per page; all chart views are rendered up front and expanded; controls and tooltips are hidden.
- **Excludes**: real customer names, account data, internal system names, real transaction volumes, and unconfirmed third-party brand marks. Sample data must be labelled "sample".
- **Pitch mode adds**: one declarative company-purpose sentence; a bottom-up market size with its denominator stated; competition including the "do nothing" alternative; the weakest point addressed on its own slide; an ask with amount, use and milestone; and a close that repeats the thesis. No third-party framework names, logos, colours, or upstream code appear in the deliverable.
- **Brand mark**: at most one HSBC Hexagon redraw in the top-right corner, following the colour, size, and placement rules in "Brand Mark". When the user explicitly wants no mark, set `<html>`'s `data-brand` to `hide`.
- **Language**: all visible copy, labels, captions, and sample content are in English.

## 6. Content discipline

- Do not rewrite, cut, or add to the user's points and conclusions. If a subject is missing or wording is unclear, ask once; do not polish it into a different meaning.
- Do not invent numbers. Leave the position empty and mark it TBD; never deliver the shell's sample figures as if they were real.
- Do not add slides unilaterally, including framework slides the user's outline does not call for. Ask before adding a "Summary" or "Q&A" slide.
- Present facts, judgements, and recommendations as the user wrote them; never add inferential phrases such as "this shows that" or "the results prove".
- Break long sentences only by line or hierarchy, never by meaning; keep technical terms as-is rather than simplifying them.
- In pitch mode, never silently reorder a supplied outline to match a framework, never blend two framework orders, and never turn an unverified figure into a market-size claim.

## 7. Visual and interaction notes

- Red is for focal points only: progress bar, current TOC item, one key figure, one bar, the latest data point, and the target line. Keep red under 10% of a screen; variance labels and status chips stay neutral unless they are the single focus.
- Build the "Fintech feel" with whitespace, weight, tabular numerals, and a 1px grid — no gradients, glows, glassmorphism, or large radii.
- Angular detailing is limited to structural chamfers and one narrow angled band on section dividers. Never tile or repeat the Hexagon as decoration.
- Transitions are 120–200ms, fade and vertical displacement under 8px only; nothing autoplays or auto-advances.
- Per-slide ceiling: one main point plus no more than 5 supporting items. Split the slide rather than shrinking type.
- Replace every piece of shell sample content; do not leave the word "sample" in the deliverable unless it genuinely labels sample data.
- Keep the brand mark fixed in the top-right corner: no animation, no recolouring, never enlarged enough to compete with the title. A white badge holds it on dark covers.
- Controls never fight paging: elements carrying `data-no-paging` swallow arrow keys and Space so a focused control does not flip the slide.
- Pitch charts (TAM funnel, waterfall, tornado, 2x2, traction) obey the same black / white / grey palette and the one-red-focal rule in `@references/visual-system.md`. A framework's original colours are never imported.

## 8. Fallbacks and stop conditions

| Situation | Behaviour |
|---|---|
| Cannot write an HTML file, or no rendering environment | Output a structured Markdown slide outline (slide / title / layout / content), marked "not rendered", with recovery steps |
| Content appears to contain non-public data | Stop, list the specific items, and ask for redaction before continuing; do not write the original text into the output |
| Verified chart figures are missing | Use a data table, mark the position TBD, or omit the chart. Never ship the shell's sample numbers |
| User asks to reproduce an upstream pitch template or deck verbatim | Decline the verbatim copy; offer a structural reinterpretation under this Skill's visual system |
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
- [ ] **Pitch mode**: framework stated, action titles read as sentences, market denominator shown, weakest point addressed, ask states amount / use / milestone, no third-party names or assets in the deck
- [ ] **Pitch mode**: the framework's acceptance list in `@references/pitch-frameworks.md` passes

Fix anything that fails before delivering; never ship a known issue.

## 10. Handover

Give the file path, total slide count, mode and framework (if any), and one line per slide describing its layout and interaction, then one line of operating instructions:

`← → to navigate · Esc for overview · N notes · F fullscreen · ? shortcuts · on-slide controls are clickable and keyboard reachable · print directly to export PDF`

No extra documentation, and no production notes or status labels inside the HTML.
