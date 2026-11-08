# Commercial Pitch Frameworks — Sources, Licences and Mappings

This reference lets `fintech-presentation` build a **commercial / investor pitch** or an **internal business case** using established open frameworks, without importing any third-party code, fonts, artwork or runtime. It supplies structure and layout vocabulary only. Every deliverable is still one self-contained HTML file with zero external requests, following `visual-system.md` and `layout-and-interaction.md`.

Sources were checked on **2026-11-08**. Nothing in this repository was retrieved from behind a login, and no upstream file is vendored.

## 1. Source register

| # | Framework | Source | Status | Borrowed into this Skill | Not borrowed |
|---|---|---|---|---|---|
| A | Sequoia "Writing a Business Plan" | `https://www.sequoiacap.com/article/writing-a-business-plan/` | Publicly published method, not a code licence | The 10-part narrative order, the single-sentence company purpose, the explicit "Why now?" slide | Sequoia's name in deliverables, any template artwork, any claim of endorsement |
| B | Y Combinator seed deck / Michael Seibel's seven questions | YC public founder guidance and the widely documented seed-deck order | Publicly published method | Traction on slide 4, action-title headlines, ~15-slide ceiling, "founders only" team slide, the specific ask | YC branding, application materials, any internal batch document |
| C | Presentable — Enterprise / Investor | `https://github.com/haiwakatta/presentable` | **MIT** | Pyramid Principle, action titles, SCR (Situation–Complication–Resolution), MECE, 2x2 / TAM-SAM-SOM / waterfall / tornado / financial-tile *chart types*, and the enterprise-vs-investor case split | All CSS, HTML, JS, `AGENTS.md` text, fonts (Fraunces, Manrope, Sora, Bricolage), colours, theme builders and CLI scripts |
| D | Opinionated 10-slide pitch deck | `https://github.com/batkotron/pitch-deck-template` | **MIT** | The 10-slide order, the "traction is the slide investors flip to first" principle, bottom-up market sizing, and the pre-send checklist as a delivery gate | `SLIDES.md` / `COMMENTARY.md` / `CHECKLIST.md` text, the Google Slides file, the fictional Australian startup examples |
| E | Kawasaki 10 / 20 / 30 | Publicly published rule of thumb | Publicly published method | The constraint itself: ~10 slides, ~20 minutes, no type below a 30pt-equivalent, one claim per slide | Deck templates sold by third parties, any branding |
| F | Airbnb (2009) and Buffer (2011) seed decks | Publicly circulated example decks | Public case studies | Sentence-length headlines, numbers instead of adjectives, four-number unit economics, past milestones and future targets on one line | All deck artwork, company names in deliverables, figures and claims |

Two further MIT projects are recorded as prior art for the *single-file HTML deck* approach and are not used as sources of structure: `https://github.com/zarazhangrui/beautiful-html-templates` (MIT) and `https://github.com/SurgePix/ai-ppt-prompts` (MIT). No file from either is bundled.

**Attribution rule.** MIT permits use without attribution; this repository credits the two MIT projects anyway in `README.md`. A public method is cited as a method, never as a licence or an endorsement. Deliverables must not carry third-party names, logos or "based on" claims. If a user asks to reproduce an upstream deck verbatim, decline and offer a structural reinterpretation.

## 2. Framework A — Sequoia investor narrative (10 slides)

Use when the audience allocates capital or sponsors a new venture and the story must prove timing, market and defensibility.

| # | Purpose | Action-title test | Layout | Interaction |
|---|---|---|---|---|
| 01 | Company purpose | One declarative sentence, no feature list | `cover` + `thesis` | none |
| 02 | Problem | Name who hurts and how it is handled today | `problem-solution` | selectable pain rows map to the matching response |
| 03 | Solution | State the insight, not the feature set | `split` | none |
| 04 | Why now | Name the change that made this possible now | `why-now` | three-signal timeline |
| 05 | Market potential | Bottom-up, with the denominator stated | `tam` | bottom-up / top-down segmented switch |
| 06 | Competition and alternatives | Include "do nothing" and internal workarounds | `competition` | selectable 2x2 quadrant with detail panel |
| 07 | Business model | Who pays, how much, how often | `business-model` | pricing-basis segmented switch |
| 08 | Team | Founders and key people, with one proof line each | `team` | none |
| 09 | Financials | Only figures the user supplied; TBD otherwise | `waterfall` | scenario segmented switch |
| 10 | Vision | What exists in five years if this works | `closing` + `ask` | expandable milestone |

## 3. Framework B — YC traction-first seed (8–10 slides)

Use when there is real momentum and the numbers should carry credibility before the market story.

| # | Purpose | Layout | Interaction |
|---|---|---|---|
| 01 | Name + one plain-language line | `cover` | none |
| 02 | Problem and its real-world impact | `problem-solution` | selectable pain rows |
| 03 | Solution in as few words as possible | `split` | none |
| 04 | **Traction, straight after the solution** | `traction` | metric segmented switch + target toggle + hover / focus tooltips |
| 05 | Supporting metrics | `kpi-row` + `unit-economics` | sparkline KPI strip |
| 06 | Unique insight / why this team | `thirds` | none |
| 07 | Business model | `business-model` | pricing-basis switch |
| 08 | Market size | `tam` | bottom-up / top-down switch |
| 09 | Team — founders only | `team` | none |
| 10 | The ask — amount and the milestone it buys in 12 months | `ask` | expandable use-of-funds |

Rules inherited from this framework: headline every slide with a full sentence, keep the presented deck to about 15 slides, put the rest in an appendix, and never include an advisor slide in the body.

## 4. Framework C — Enterprise business case (11–13 slides)

Use when the audience is an executive, board, CFO or investment committee and the output must support an approval decision. This is the closest fit for internal platform, FinOps and migration proposals.

| # | Purpose | Layout | Interaction |
|---|---|---|---|
| 01 | Cover with the decision being asked for | `cover` | none |
| 02 | Executive summary (Pyramid Principle: answer first) | `thesis` + `kpi-row` | none |
| 03 | Situation – Complication – Resolution | `scr` | selectable column detail |
| 04 | Baseline and current cost | `chart-bar` | period segmented switch |
| 05 | Options assessment, MECE and identically scored | `option-matrix` | option tabs + scored matrix |
| 06 | Recommended option and trade-offs | `split` | none |
| 07 | Benefit waterfall / cost-benefit bridge | `waterfall` | scenario segmented switch |
| 08 | Financial tiles: NPV, IRR, payback, ROI | `kpi-row` + `table` | sortable table |
| 09 | Sensitivity / tornado | `tornado` | driver segmented switch |
| 10 | Delivery roadmap with exit conditions | `roadmap` | selectable phases |
| 11 | Risk register and controls | `sortable-table` + `levers` | sorting + expandable rows |
| 12 | The ask, named owner and decision date | `ask` | expandable milestones |
| 13 | Appendix pointer / closing | `closing` | none |

Consulting discipline borrowed from Presentable: write the **action title first** and make the headline readable on its own; group supporting points MECE; state the recommendation before the evidence; keep technical detail in the appendix. Do not adopt Presentable's fonts, deep-blue accent, warm portfolio palette or orange investor accent — `visual-system.md` is authoritative.

## 5. Framework D — Opinionated 10-slide pitch (10 slides)

Use when the user wants the shortest credible commercial story and a hard pre-send gate.

Order: Title → Problem → Solution → Market → Product → Traction → Business model → Competition → Team → Ask.

Two rules make this framework worth keeping: market size must be built bottom-up from a stated denominator, and traction is treated as the slide the reader jumps to first, so it must stand alone if read out of order. Run the pre-send checklist before delivery:

- [ ] Every slide makes one claim, and the claim is the headline
- [ ] Company purpose fits on one line with no jargon
- [ ] Market sizing shows its denominator, not just a total
- [ ] Traction page is legible with no spoken explanation
- [ ] Competition includes the "do nothing" alternative
- [ ] The ask states the amount, the use and the milestone
- [ ] Every figure has a source; unverified figures are tables or `TBD`
- [ ] No real customer names, internal system names or unpublished metrics

## 6. Framework E — Kawasaki 10 / 20 / 30 as a constraint

This is not a layout. Apply it as a ceiling after the slide plan is drafted:

- roughly 10 presented slides, 20 minutes, and no body type below the 30pt-equivalent;
- if a slide needs smaller type to fit, split the slide instead of shrinking;
- deep detail moves to an appendix, never onto a dense body slide;
- the meeting keeps at least half its time for questions.

For a 16:9 container-scaled deck, the 30pt-equivalent is approximately `1.7cqw` for body copy. Headlines in `visual-system.md` already exceed it; tables and chart axis labels are the exception because they are data, not prose.

## 7. Choosing a framework

| Signal | Framework |
|---|---|
| External investors, seed / Series A, capital allocation | A or B |
| Real traction exists and should lead | B |
| Board / CFO / executive approval, internal investment, FinOps or platform proposal | C |
| Shortest possible commercial story, or a pre-send review | D |
| User fixes a hard time or slide budget | E as an overlay on A, B, C or D |
| User supplies an existing outline and says "keep my order" | None — keep the user's order, use only the layout library |

When two frameworks fit, ask one question about the decision the audience is being asked to make. Never silently blend two orders. Never add a framework's slides to a user-supplied outline without asking.

## 8. Layout and data mapping

Layouts are defined in `layout-and-interaction.md`; renderers and data keys live in `assets/pitch-deck-shell.html`.

| Layout | Renderer | Data key | Control |
|---|---|---|---|
| `thesis` | static | — | — |
| `problem-solution` | `renderProblem()` | `PAINS` | `data-choice="pain"` |
| `why-now` | static | — | — |
| `tam` | `renderTam()` | `MARKET` | `data-seg="tam"` |
| `traction` | `renderTraction()` | `TRACTION` | `data-seg="traction"`, `targetToggle` |
| `unit-economics` | `renderUnits()` | `UNITS` | — |
| `competition` | `renderComp()` | `RIVALS` | `data-choice="rival"` |
| `business-model` | `renderPricing()` | `PRICING` | `data-seg="pricing"` |
| `waterfall` | `renderWaterfall()` | `SCENARIOS` | `data-seg="scenario"` |
| `tornado` | `renderTornado()` | `DRIVERS` | `data-seg="driver"` |
| `team` | static | — | — |
| `ask` | `renderAsk()` | `ASK` | `data-expand` |
| `scr` | static + `data-choice="scr"` | `SCR` | selectable column |

Every chart is inline SVG generated by the file's own script. No chart library, no CDN, no `fetch`, no external `<use>` target. All interactive marks carry `data-tip`, `data-tip-v` and `tabindex="0"`. Every control group carries `data-no-paging`.

## 9. Pitch-specific content discipline

- The company purpose is **one declarative sentence**. If it needs a semicolon or a feature list, ask the user to restate it.
- Headlines are full sentences that carry the conclusion. Reading only the headlines must still tell the story.
- Numbers replace adjectives. If a claim has no number, do not add a fake one; write `TBD: <what is needed>`.
- Market size is bottom-up: state the unit, the count of units and the price, and show the denominator. Never write "a $X billion market" without a derivation.
- Competition includes indirect alternatives and doing nothing.
- Name the weakest point on its own slide rather than hiding it. Do not answer it with an inferential phrase such as "this shows that".
- The ask states the amount, the use of funds and the milestone with a date; the close repeats the thesis.
- All figures in a sample deck are fictional and labelled "sample". Replace or delete them before any real use. A verified figure is one the user supplied, not one a framework suggests.
- Never rewrite the user's conclusions, and never add a recommendation the user did not make.

## 10. Acceptance for a pitch deck

- [ ] A framework was chosen and stated before the slide plan, or the user's own order was kept
- [ ] Slide / title / layout / interaction / source-section mapping table was reviewed first
- [ ] One claim per slide; no two consecutive slides share a layout
- [ ] Action titles read as sentences; the thesis and the ask are both one line
- [ ] Framework D's pre-send checklist passes when that framework is used
- [ ] Framework E's type ceiling is respected when that overlay is used
- [ ] Market sizing states its denominator; the weakest point is addressed
- [ ] All figures either user-supplied, `TBD`, or labelled "sample"
- [ ] Each segmented control, toggle, selectable panel and expandable row was exercised
- [ ] Zero external requests; print expands every chart view; keyboard reaches every control
- [ ] No third-party names, logos, colours or upstream code appear in the deliverable
- [ ] All visible copy is in English
