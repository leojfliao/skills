# Layout Library and Interaction Rules

The shell `assets/deck-shell.html` is a **working 14-slide example deck**, not an empty template. It already implements the CSS variables, chart renderers, control delegation, keyboard map, speaker notes, overview, print expansion, and brand mark. This file explains which layout to choose, how to write each structural block, how the interactive components are wired, and which pitfalls will ruin a slide.

Start by replacing the sample content and sample data, then delete the slides you do not need. Do not rewrite the shell's machinery.

## 1. Layout Library

### Base layouts (static)

| Layout | Use For | Per-Slide Limit |
|---|---|---|
| `cover` Cover | Opening, title + one key claim | 1 title, 1 subtitle line |
| `thirds` Three-Column Cards | Three parallel capabilities, phases, or risk types | 3 cards, 2 lines each |
| `metric` Metric Slide | A conclusion carried by a single number | 1 main number + 3 supporting metrics |
| `split` Split Comparison | Current vs. target, plan A vs. B, before/after | 4 items per side |
| `timeline` Timeline | Phase progression, milestones, evolution path | 4 nodes |
| `figure` Figure Slide | Embedding architecture, solution, or flow diagrams | 1 figure + 1 caption line |
| `table` Data Table | Exact values needing item-by-item comparison | 6 rows × 5 columns |
| `closing` Closing | One-line conclusion, next steps, Q&A | 1 line + 3 action items |

### Professional layouts (in the shell)

| Layout | Use For | Per-Slide Limit |
|---|---|---|
| `divider` Section Divider | Chapter break; dark ground, big number, one angled red band on the left | 1 number, 1 title, 1 subtitle |
| `agenda` Clickable Agenda | Four sections with jump targets | 4 cards, each with `data-goto` |
| `kpi-row` KPI Strip | Four compact figures with sparkline support | 4 KPIs, 1 sparkline among them |
| `chart-bar` Grouped Bars | FinOps budget vs. actual by category | 6 categories, 2 series, 1 red focal bar |
| `chart-line` Trend Line | 12-month service metric with target line | 12 points, 1 target line, 1 red latest point |
| `levers` Expandable Rows | Savings levers, control details, ranked actions | 3–4 rows, 2 detail fields each |
| `sortable-table` Sortable Table | SLA posture, risk register | 6 rows × 5 columns, text or numeric sort |
| `option-matrix` Option Comparison | Three options scored on identical dimensions | 5 dimensions, 3 option columns |
| `arch-select` Selectable Architecture | Four tiers with a selectable focal path | 4 tiers × 3 components, 1 red path |
| `roadmap` Selectable Roadmap | Four phases with exit conditions | 4 phases, 1 detail panel |

Do not use the same layout on two consecutive slides. When three consecutive slides are all bullet lists, change the middle one to a KPI strip or a split comparison. A `divider` may precede a content slide of any layout.

**Shell page map**: 01 cover · 02 agenda · 03 divider · 04 chart-bar · 05 levers · 06 divider · 07 chart-line · 08 sortable-table · 09 divider · 10 option-matrix · 11 arch-select · 12 roadmap · 13 sortable-table + levers · 14 closing.

## 2. Structure Snippets

All snippets go inside `<section class="slide" data-title="Slide Title">`. `data-title` must be non-empty; the table of contents and overview are generated from it. `data-chapter` groups the overview, and `<template class="notes">` feeds the `N` notes drawer.

```html
<section class="slide" data-title="Cost vs. budget" data-chapter="FinOps">
  ...
  <template class="notes"><b>Key point.</b> One line on what to say here.</template>
</section>
```

### Cover

```html
<section class="slide cover" data-title="Cover" data-chapter="Start">
  <p class="eyebrow">Internal Share · X / 20XX</p>
  <h1 class="title big">One line on what we'll cover</h1>
  <p class="sub">Why it's worth your time</p>
  <p class="note">Speaker / Team · 45 min</p>
</section>
```

The shell places the brand mark in the top-right corner of the cover and every other slide; **do not write it again inside `<section>`**. This block already exists inside `.deck` and outside `.stage`:

```html
<div class="brand">
  <svg viewBox="0 5.9 24 12.2" role="img" aria-label="HSBC"><use href="#brand-hex"/></svg>
</div>
```

`<symbol id="brand-hex">` is the only mark definition. To hide it across the whole deck: `<html data-brand="hide">`; to swap in the official logo: edit only the paths inside that `<symbol>`. For geometry, dimensions, and constraints see `references/visual-system.md`, "Brand Identity"; you can preview `assets/brand-mark.svg`.

### Section Divider

```html
<section class="slide divider" data-title="01 · FinOps" data-chapter="FinOps">
  <p class="eyebrow">Section one</p>
  <div class="div-no num">01</div>
  <h2 class="title">FinOps — spend as an engineering signal</h2>
  <div class="rule"></div>
  <p class="sub">One line of framing.</p>
  <p class="note">Sample data · USD millions</p>
</section>
```

The angled red band is a `::after` pseudo-element on the left, 7cqw wide. `.slide.divider` adds `padding-left:10cqw` so text never sits on it. The shell sets `html[data-dark="true"]` and `html[data-divider="true"]` on each render: the first lightens the TOC dots and page counter, the second moves the counter clear of the band. Keep both attributes if you copy the divider.

### KPI Strip

```html
<div class="kpi-row" id="myKpi"></div>
```

KPI content is generated by the `kpi(cells)` helper. Each cell accepts `k` (label), `v` (value), `u` (unit), `d` (note), `hot` (red value), and `spark` (array of numbers). Prefer four KPIs per strip; route all numbers through `.num`.

### Charts and Controls

```html
<div class="toolbar">
  <div class="seg" role="group" aria-label="Reporting period" data-seg="finops" data-no-paging>
    <button type="button" data-view="Q1" aria-pressed="true">Q1</button>
    <button type="button" data-view="Q2" aria-pressed="false">Q2</button>
  </div>
  <button type="button" class="toggle" id="varianceToggle" aria-pressed="false" data-no-paging>
    <span class="box" aria-hidden="true">✓</span> Highlight variance
  </button>
  <span class="hint">Hover or focus a bar for exact values</span>
</div>
<div class="chart" id="finopsChart" data-chart="finops"></div>
<div class="chart-legend">
  <span><i style="background:#D7D8D6"></i>Budget</span>
  <span><i style="background:#141414"></i>Actual</span>
  <span class="cap num" id="finopsCaption">Q1 · sample</span>
</div>
<div class="print-views" data-print-views="finops"></div>
```

`data-seg` names a key in the `state` object. Clicking a button sets that state, updates every `aria-pressed` in the group, and calls the matching renderer. `data-no-paging` is mandatory on any group of controls: it stops arrow keys, Space, and Enter from flipping the slide while a control has focus. Inside a `data-seg` group, Left / Right move between buttons and activate them.

The chart container is filled by a renderer that returns a `<svg class="chart-svg" viewBox="0 0 1200 320" role="img" aria-label="…">`. Every data mark carries `data-tip` and `data-tip-v`, which the shared tooltip reads on `mouseover` and `focusin`. Give marks `tabindex="0"` so the values are reachable without a mouse. The `.cap` legend item is pushed to the far right with `margin-left:auto`; keep it short.

`data-print-views` is a hidden print-only container. `renderPrintCharts()` fills it at init with one SVG per view, so printing never depends on `beforeprint` firing.

### Expandable Rows

```html
<div class="levers" data-no-paging>
  <div class="lever">
    <button class="lever-head" type="button" data-expand="lv1" aria-expanded="false" aria-controls="lv1">
      <span class="lh-n num">01</span><span class="lh-t">Lever name</span>
      <span class="lh-v num">− 4.2 USD m / yr</span><span class="lh-i" aria-hidden="true">+</span>
    </button>
    <div class="lever-body" id="lv1" hidden><p>…</p><dl>…</dl></div>
  </div>
</div>
```

One delegated handler reads `data-expand`, toggles `hidden` and `aria-expanded`, and swaps `+` for `−`. At print time `expandForPrint()` opens every row and `restoreAfterPrint()` puts them back.

### Sortable Table

```html
<table class="data" id="slaTable">
  <thead><tr><th><button type="button" data-sort="0" data-no-paging>Service <span class="arrow">↕</span></button></th></tr></thead>
  <tbody><tr><td>Sample service</td></tr></tbody>
</table>
```

`data-sort` is the zero-based column index. `sortTable()` detects numeric cells by parsing digits, sets `aria-sort` on the header, and updates the arrow. Text columns use `localeCompare`. Keep `data-no-paging` on every header button.

### Option Matrix, Architecture, Roadmap

- `option-matrix`: the table cells carry `data-col="A|B|C"` and `data-score="1..5"`. `renderDots()` paints five squares per cell; `renderOption()` highlights one column and fills the `.opt-detail` panel. The detail `dl` is a two-column grid — do not switch `dt` / `dd` back to inline, or labels and values collide.
- `arch-select`: each `<g class="layer" data-layer="…" tabindex="0" role="button">` is clickable and keyboard activatable. `renderLayer()` updates the detail panel; `renderFlow()` toggles the red path. **Use `style.display`, not the `hidden` attribute, on SVG children** — `hidden` is unreliable inside SVG.
- `roadmap`: `data-phase` buttons set `state.phase`; `renderPhase()` fills `.road-detail`.

### Data Table

```html
<table class="data">
  <thead><tr><th>Metric</th><th>Before</th><th>After</th><th>Notes</th></tr></thead>
  <tbody><tr><td>Latency</td><td class="num">203 ms</td><td class="num">128 ms</td><td>Peak basis</td></tr></tbody>
</table>
```

Add `class="num"` to numeric columns for right alignment; left-align text columns.

### Closing

```html
<section class="slide closing" data-title="Summary">
  <p class="kicker">One line to close on</p>
  <ul class="actions"><li><b class="num">01</b>Next step 1</li></ul>
  <p class="note">Q&amp;A</p>
</section>
```

## 3. Data Contract

All chart data lives in one script object block: `MONTHS`, `CATS`, `QUARTERS`, `METRICS`, `OPTIONS`, `LAYERS`, `PHASES`, plus `state`. Renderers are pure functions of `state`:

| State key | Control | Renderer |
|---|---|---|
| `finops` | period segmented control | `renderFinops()` → `finopsSvg()` + KPI strip |
| `variance` | highlight toggle | `renderFinops()` (one red bar) |
| `metric` | series segmented control | `renderMetric()` → `metricSvg()` + KPI strip |
| `target` | show target toggle | `renderMetric()` (dashed red threshold) |
| `option` | option tabs | `renderOption()` |
| `flow` | call-path segmented control | `renderFlow()` |
| `layer` | clickable tier | `renderLayer()` |
| `phase` | roadmap phase | `renderPhase()` |

Rules:

- Replace **every** sample number with a verified figure before delivery. If verification is missing, delete the chart and use a table.
- `METRICS[key].better` is `"down"` or `"up"`. The 12-month change label must be computed from it (`improved` vs `worsened`), not from the sign of the delta.
- Status colours are not data. Variance labels and status chips stay neutral grey/black; only the single focal item turns red.
- `esc()` must be applied to any user-supplied text inserted into generated SVG or HTML.

## 4. Deck State Machine (Do Not Rewrite)

The script maintains a current-slide index and handles: toggling `.active`, progress-bar width, page number, TOC and overview highlights, URL hash, `Esc` overview grid, `N` notes drawer, `?` shortcut help, `F` fullscreen, click-to-jump, deep links via `hashchange`, and print expansion.

Adding or removing slides requires only two things: add or remove `<section class="slide">`, and keep every `data-title` non-empty. The table of contents, overview, page numbers, and progress bar all recalculate automatically; do not manually edit a slide-count constant, and do not write a second paging logic. The brand mark is not a slide and takes no part in paging.

`render()` also sets `html[data-dark]` and `html[data-divider]` from the current slide's classes. Keep these attributes: they keep the TOC and page counter readable on dark and red-banded slides.

After removing a slide, refresh once and confirm the progress bar reaches 100% on the last slide, the TOC item count matches the slide count, and every `data-goto` target still points at the intended section.

## 5. Common Pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| Cross-file SVG reference | Icons blank when opened locally by double-click | Copy the used `<symbol>` definitions into this page's `<defs>`, or inline the whole figure |
| `hidden` on SVG children | The asynchronous call path never appears | Toggle `style.display` instead |
| Slides transparent when printing | Exported PDF has only the first page or is all white | In print styles, force `opacity:1` and `position:relative` on `.slide` and clear `transform` |
| Print loses chart views | Only the currently selected chart prints | Keep `renderPrintCharts()` at init; it pre-renders every view into `[data-print-views]` |
| Controls flip the slide | Pressing Space on a button advances the deck | Put `data-no-paging` on every control group |
| Detail `dl` collides | Labels and values run together on one line | Use the two-column grid on `.opt-detail dl` |
| Table overflow | Right-hand columns get clipped | Drop columns or split the slide; do not shrink the type size |
| Legend collides with caption | Long legend text touches the period label | Give the caption `.cap` so `margin-left:auto` pushes it clear |
| Numbers jitter | Values shift left and right when paging | Route all numbers through tabular numerals and `tabular-nums` |
| Too much red | Slide looks like a promo poster | One focal series only; neutral variance labels, neutral status chips, one narrow divider band |
| English copy overflows | Labels that fitted in Chinese now clip | Prefer short labels; check the longest category / option name at 16:9 |
| Gray text on dark slides | Weak contrast on reversed slides | Use pure white text; lighten HUD and TOC via `html[data-dark="true"]` |
| Focus lost | Arrow keys do nothing after a keyboard jump | After a jump, hand focus back to the slide container, not the button |
| Chart with no data | Axes drawn but no points, still takes a full slide | With no verified values, switch to a data table or delete the slide |
| TOC out of sync | Old titles show up in the overview | Write titles only in `data-title`; do not keep a second copy in the script |
| Mark distorted or missing | Stretched into a rectangle, center white hourglass blurs, all white when opened locally | Keep the 24 : 12.2 ratio, width at least 32px, and `<use>` only referencing `#brand-hex` in the same document |

## 6. Acceptance

- [ ] Every layout is used correctly at least once, or a reason is given for skipping it
- [ ] No two consecutive slides share the same layout
- [ ] One main point per slide, with no more than 5 supporting items
- [ ] All inline SVGs have `viewBox` and accessible labels
- [ ] Every segmented control, toggle, expandable row, sortable header, and detail panel was exercised
- [ ] Chart tooltips appear on hover and on keyboard focus
- [ ] All shell sample data replaced with verified figures, or charts removed
- [ ] Print preview checked slide by slide, with no clipping and all chart views expanded
- [ ] Full keyboard flow works end to end, with focus always visible
- [ ] At most one brand mark per slide, correct ratio, appears on every page when printing
- [ ] Red stays under 10% of every screen
