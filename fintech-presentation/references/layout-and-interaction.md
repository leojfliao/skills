# Layout Library and Interaction Rules

The shell `assets/deck-shell.html` already implements all CSS variables and the interaction state machine. This file explains which layout to choose, how to write each structural block, and which pitfalls will ruin a slide.

## 1. Layout Reference

| Layout | Use For | Per-Slide Limit |
|---|---|---|
| `cover` Cover | Opening, title + one key claim | 1 title, 1 subtitle line |
| `thirds` Three-Column Cards | Three parallel capabilities, three phases, three risk types | 3 cards, 2 lines each |
| `metric` Metric Slide | A conclusion carried by a single number | 1 main number + 3 supporting metrics |
| `split` Split Comparison | Current vs. target, plan A vs. B, before/after | 4 items per side |
| `timeline` Timeline | Phase progression, milestones, evolution path | 4 nodes |
| `figure` Figure Slide | Embedding architecture, solution, or flow diagrams | 1 figure + 1 caption line |
| `table` Data Table | When exact values need item-by-item comparison | 6 rows × 4 columns |
| `closing` Closing | One-line conclusion, next steps, Q&A | 1 line + 3 action items |

Do not use the same layout on two consecutive slides. When three consecutive slides are all bullet lists, change the middle one to a metric slide or a split comparison.

## 2. Structure Snippets

All snippets go inside `<section class="slide" data-title="Slide Title">`. `data-title` must be non-empty; the table of contents and overview are generated from it.

### Cover

```html
<section class="slide cover" data-title="Cover">
  <p class="eyebrow">Internal Share · X / 20XX</p>
  <h1 class="title">One line on what we'll cover</h1>
  <p class="sub">Why it's worth your time</p>
  <p class="note">Speaker / Team · 45 min</p>
</section>
```

The shell places the brand mark in the top-right corner of the cover and every other slide; **do not write it again inside `<section>`**. This block already exists inside `.deck` and outside `.stage` in `deck-shell.html`:

```html
<div class="brand">
  <svg viewBox="0 5.9 24 12.2" role="img" aria-label="HSBC"><use href="#brand-hex"/></svg>
</div>
```

`<symbol id="brand-hex">` is the only mark definition. To hide it across the whole deck: `<html data-brand="hide">`; to swap in the official logo: edit only the paths inside that `<symbol>`. For geometry, dimensions, and constraints see `references/visual-system.md`, "Brand Identity"; you can preview `assets/brand-mark.svg`.

### Three-Column Cards

```html
<section class="slide" data-title="Three Capabilities">
  <h2 class="title">Three things this plan solves</h2>
  <div class="grid-3">
    <div class="card"><span class="idx num">01</span><h3>Capability One</h3><p>One line of copy, two lines max.</p></div>
    <div class="card"><span class="idx num">02</span><h3>Capability Two</h3><p>One line of copy, two lines max.</p></div>
    <div class="card"><span class="idx num">03</span><h3>Capability Three</h3><p>One line of copy, two lines max.</p></div>
  </div>
</section>
```

### Metric Slide

```html
<section class="slide" data-title="Results">
  <h2 class="title">The clearest change after the rebuild</h2>
  <div class="metric">
    <span class="value num">128<span class="unit">ms</span></span>
    <span class="label">End-to-end latency</span>
    <span class="delta">Down <b class="num">37%</b> vs. before</span>
  </div>
  <div class="metric-row">
    <div><span class="num">3</span><span class="label">Connected systems</span></div>
    <div><span class="num">99.9%</span><span class="label">Availability</span></div>
    <div><span class="num">0</span><span class="label">Manual interventions</span></div>
  </div>
</section>
```

When a number has no verified source, do not fill it with a placeholder figure; leave the spot empty and mark it as TBD.

### Split Comparison

```html
<section class="slide" data-title="Before vs. After">
  <h2 class="title">Before and after</h2>
  <div class="split">
    <div class="col"><h3>Current</h3><ul><li>Key point</li><li>Key point</li></ul></div>
    <div class="col accent"><h3>Target</h3><ul><li>Key point</li><li>Key point</li></ul></div>
  </div>
</section>
```

Only the right-hand `.accent` column gets a thin red border or a red subheading; never a full red background.

### Timeline

```html
<section class="slide" data-title="Rollout Cadence">
  <h2 class="title">Four steps to delivery</h2>
  <div class="timeline">
    <div class="tl-item"><span class="when num">Q1</span><h3>Map</h3><p>One-line note</p></div>
    <div class="tl-item"><span class="when num">Q2</span><h3>Pilot</h3><p>One-line note</p></div>
    <div class="tl-item"><span class="when num">Q3</span><h3>Scale</h3><p>One-line note</p></div>
    <div class="tl-item"><span class="when num">Q4</span><h3>Wrap Up</h3><p>One-line note</p></div>
  </div>
</section>
```

### Figure Slide

```html
<section class="slide" data-title="System Architecture">
  <h2 class="title">Overall layering</h2>
  <figure class="figure">
    <svg viewBox="0 0 1200 480" role="img" aria-label="System layering diagram">…inline paths…</svg>
    <figcaption>Figure 1 · System layers and call direction (sample)</figcaption>
  </figure>
</section>
```

SVG must be inlined in full, with `viewBox` and `role="img"` / `aria-label`. Do not write `<img src>` or `<use href="external-file#...">`: browsers often block cross-file references under `file://`.

### Data Table

```html
<section class="slide" data-title="Metric Comparison">
  <h2 class="title">Three Metrics</h2>
  <table class="data">
    <thead><tr><th>Metric</th><th>Before</th><th>After</th><th>Notes</th></tr></thead>
    <tbody><tr><td>Latency</td><td class="num">203 ms</td><td class="num">128 ms</td><td>Peak basis</td></tr></tbody>
  </table>
</section>
```

Add `class="num"` to numeric columns for right alignment; left-align text columns.

### Closing

```html
<section class="slide closing" data-title="Summary">
  <p class="kicker">One line to close on</p>
  <ul class="actions"><li>Next step 1</li><li>Next step 2</li><li>Next step 3</li></ul>
  <p class="note">Q&amp;A</p>
</section>
```

## 3. Deck State Machine (Do Not Rewrite)

The script in `deck-shell.html` maintains a current-slide index and handles: toggling `.active`, updating progress-bar width, updating the page number, syncing the TOC highlight, writing the URL hash, the `Esc` overview grid, click-to-jump, and print expansion.

Adding or removing slides requires only two things: add or remove `<section class="slide">`, and keep every `data-title` non-empty. The table of contents, overview, page numbers, and progress bar all recalculate automatically; do not manually edit the slide-count constant in the script, and do not write a separate paging logic. The brand mark is not a slide: it takes no part in paging and is excluded from the table of contents and overview.

After removing a slide, refresh once and confirm the progress bar reaches 100% on the last slide and the TOC item count matches the slide count.

## 4. Common Pitfalls

| Pitfall | Symptom | Fix |
|---|---|---|
| Cross-file SVG reference | Icons blank when opened locally by double-click | Copy the used `<symbol>` definitions into this page's `<defs>`, or inline the whole figure |
| Slides transparent when printing | Exported PDF has only the first page or is all white | In print styles, force `opacity:1` and `position:relative` on `.slide` and clear `transform` |
| Table overflow | Right-hand columns get clipped | Drop columns or split the slide; do not fix it by shrinking the type size |
| Numbers jitter | Values shift left and right when paging | Route all numbers through tabular numerals and `tabular-nums` |
| Too much red | Slide looks like a promo poster | Keep a single red focus; push everything else to black, white, and gray |
| Gray text on dark slides | Weak contrast on reversed slides | Use pure white text on dark backgrounds; notes no darker than `#D7D8D6` |
| Focus lost | Arrow keys do nothing after a keyboard jump | After a jump, hand focus back to the slide container, not the button |
| Ugly line breaks in Chinese | The last character of a title strands on its own line | Keep titles under 18 characters, or break manually at a semantic point with `<br>` |
| Chart with no data | Axes drawn but no points, still takes a full slide | With no verified values, switch to a data table or delete the slide |
| TOC out of sync | Old titles show up in the overview | Write titles only in `data-title`; do not keep a second copy in the script |
| Mark distorted or missing | Stretched into a rectangle, center white hourglass blurs, all white when opened locally | Keep the 24 : 12.2 ratio, width at least 32px, and `<use>` only referencing `#brand-hex` in the same document |

## 5. Acceptance

- [ ] Every layout is used correctly at least once, or a reason is given for skipping it
- [ ] No two consecutive slides share the same layout
- [ ] One main point per slide, with no more than 5 supporting items
- [ ] All inline SVGs have `viewBox` and accessible labels
- [ ] Print preview checked slide by slide, with no clipping
- [ ] Full keyboard flow works end to end, with focus always visible
- [ ] At most one brand mark per slide, correct ratio, appears on every page when printing
