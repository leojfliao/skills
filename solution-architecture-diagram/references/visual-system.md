# Fintech Presentation & Diagram Visual System

## What this document is

This is not an official HSBC brand guideline, nor material published by HSBC. It is a set of internal presentation and diagramming conventions distilled from publicly verifiable brand characteristics, shared by the three Skills in this repository so that decks and diagrams look like they belong to the same system.

- Verified on: 2026-11-08
- Official design system: `https://create.hsbc/` — the home page and `/external/newsfeed/*` articles are public; the brand asset library, guidelines, toolkits, and official logo files require a login (`https://create.hsbc/login.html`: one-click inside the network, or email + password outside). This repository contains nothing obtained from behind that login.
- Publicly citable official brand principles (`https://create.hsbc/external/newsfeed/brand-identity.html`): Make it proudly HSBC ("there's only one HSBC logo"), Use distinctive assets, Embrace our angles, Lead with red, Express internationalism, Showcase our open, inclusive and human values.
- Public references:
  - `https://brandpalettes.com/hsbc-colors/` — primary colour and black/white values
  - `https://www.designsystems.one/design-systems/hsbc-design` — colour tokens and type scale; the site describes itself as "non-official, indicative values"
  - `https://www.designyourway.net/blog/hsbc-logo/` — wordmark history; the article states explicitly that HSBC has not published a public brand guideline
  - `https://www.hsbc.com/who-we-are/our-strategy-and-values/our-history` — the Hexagon was designed by Henry Steiner in 1983, inspired by the Saltire
- Consistency note: mainstream sources give the primary red as `#DB0011`; a few third-party sites give `#DA0011`. This system uses `#DB0011` throughout and never mixes the two.
- Compliance boundary: **decks may carry one controlled redraw of the HSBC Hexagon** (see "Brand Mark"); no other third-party brand marks or vendor logos are introduced. No commercial fonts are bundled and no real customer or business data is written. Keep the "non-official" notice on public display.
- Language: all prose, labels, sample copy, and generated deliverables are in English.

## Colour

| Use | Value | Notes |
|---|---|---|
| Brand red / primary accent | `#DB0011` | Focal points, progress, primary buttons, key figures only |
| Pure white / page | `#FFFFFF` | Default background |
| Pure black / headings | `#000000` | Top-level headings and icon strokes |
| Body text | `#333333` | Default text |
| Secondary text | `#767676` | Notes, legends, labels |
| Light grey surface | `#F4F4F4` | Card backgrounds, zebra striping |
| Divider | `#D7D8D6` | 1px borders, grid lines |
| Dark surface | `#141414` | Reversed cover and section pages; a design extension, not a brand colour |
| Positive status | `#008580` | Success, achieved |
| Negative status | `#A8000B` | Error, risk |

Rules:

- Red is scarce. Keep red coverage under 10% of any screen (the brand mark counts toward this). Build hierarchy with whitespace and weight, not large colour blocks.
- No gradients, glows, neon, or glassmorphism. The "Fintech cool" comes from data density, precise alignment, and tabular numerals — not effects.
- Repeating linear gradients are allowed only to draw 1px grid lines (opacity no higher than 0.4), never as a colour transition.
- White on red is about 5.2:1 (passes AA); red text on white is only for short headings of at least 24px, never body copy.
- Body contrast is at least 4.5:1, large text at least 3:1.
- Charts stay black / white / grey; red marks a single focal series. Multiple series are separated by grey luminance, never a rainbow palette.

## Brand Mark (decks only)

`fintech-presentation` may place one HSBC Hexagon at a fixed position on the deck. This is a **redraw, not an official HSBC asset**. `solution-architecture-diagram` and `internal-share-narrative` outputs contain no brand mark by default.

| Item | Rule |
|---|---|
| Geometry | Horizontally stretched hexagon of four red triangular wedges tip-to-tip, with a white hourglass negative space at the centre; ratio 24 : 12.2 |
| Colour | `#DB0011` on white only; never reversed, recoloured, outlined, or shadowed |
| Position | Top-right of the deck, held by a small white badge so dark covers and light content pages behave identically |
| Size | About 2.7cqw wide, 32px minimum; below 32px the central negative space is lost |
| Count | At most one per page; never repeated, tiled, used as a watermark, or placed inside a chart |
| Area | Counts toward the 10% red ceiling for that screen |

Implementation: `fintech-presentation/assets/brand-mark.svg` is the previewable source of truth for the geometry; `assets/deck-shell.html` inlines `<symbol id="brand-hex">` and every page references it with `<use href="#brand-hex">` (same-document reference, never cross-file).

- **Swap in the official asset**: sign in to `https://create.hsbc/`, download the official logo SVG, and replace only the path inside `<symbol id="brand-hex">`. Nothing else on the page needs to change.
- **Remove the mark**: change `<html>` from `data-brand="show"` to `"hide"`, or delete the `.brand` element.
- **Prohibited**: stretching, rotating, recolouring, rounded corners or drop shadows, combining with text into a "new logo", use as a background pattern, anything below 32px, and placement in architecture or data diagrams.
- **Trademark notice**: the HSBC name and Hexagon mark belong to HSBC Holdings plc. The official article states "there's only one HSBC logo", so this redraw is for internal layout illustration only and must not be presented externally as the official mark. Formal or external material must use the official asset obtained after login.
- **Source and licence**: geometry follows the publicly visible mark; path data is referenced from the Simple Icons HSBC icon (CC0 1.0) and has been render-verified. The other 47 architecture icons remain hand-drawn; no third-party icon library is bundled.

## Typography

```css
--font-sans: 'Univers Next', 'Nimbus Sans', Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', Arial, Helvetica, sans-serif;
--font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, 'Liberation Mono', monospace;
```

- Fonts are neither bundled nor loaded remotely. `Univers Next` is a commercial custom typeface; when absent the stack falls back to system sans-serif without changing the layout.
- All numbers, amounts, percentages, and timestamps use the monospace stack with `font-variant-numeric: tabular-nums` so columns align and values do not shift.
- No CJK font family is specified; any non-Latin text falls back to the system.
- Do not compose an "HSBC" wordmark out of system fonts to imitate the official lockup. Use the official asset file if the full lockup is needed.

## Type Scale and Line Height

| Level | Deck | Diagram | Line height |
|---|---|---|---|
| Main title | 44–56px | 20–24px | 1.15 |
| Section title | 28–36px | 16–18px | 1.25 |
| Subheading | 20–24px | 13–14px | 1.35 |
| Body | 16–20px | 12–13px | 1.6 |
| Note | 13–14px | 10–11px | 1.5 |

- Decks are designed at 1920×1080 / 16:9 and scale with container units or `clamp()`.
- Diagram text is never smaller than 10px and stays readable after scaled export.

## Spacing and Shape

- 4px base unit; use only 4 / 8 / 16 / 24 / 32 / 48 / 64.
- Corner radius 0–2px. No large radii, no stacked shadows, at most one 1px border.
- Grid lines are 1px `#D7D8D6`; a very light background grid is acceptable for a technical feel.
- Alignment beats decoration: left edges share a baseline, numbers are right-aligned.
- The official "Embrace our angles" principle permits a small amount of hexagonal chamfer detailing, but the mark itself must never be repeated as a decorative pattern.

## Motion

- Transitions are 120–200ms `ease-out` and limited to: fade in, vertical displacement of no more than 8px, number rolls, progress-bar length changes.
- Prohibited: bounce, rotation, parallax, auto-carousel, autoplaying animation, cursor-following effects. The brand mark never animates or rotates.
- Motion must respond to user action, never start on its own; `prefers-reduced-motion: reduce` must disable every transition.
- Printing or PDF export disables all animation and expands all content.

## Interaction (decks only)

| Interaction | Behaviour |
|---|---|
| `→` / `Space` / `PgDn` | Next slide |
| `←` / `PgUp` | Previous slide |
| `Home` / `End` | First / last slide |
| `Esc` | Toggle overview grid |
| TOC click | Jump to a section |
| Progress bar | Shows position, click to jump |
| Chart data point | Hover / focus reveals the value without covering body text |

- Every interaction is keyboard reachable, with a visible focus outline (2px `#DB0011`).
- Interaction uses no external library, CDN, or network request; the single file runs offline.
- The brand mark is not a control: it takes no focus and binds no events.

## Prohibited List

1. Apart from the deck-only Hexagon redraw defined under "Brand Mark", introduce no third-party brand marks, vendor logos, or distorted marks.
2. No real customer names, account data, internal system names, or real transaction volumes. Samples use neutral fictional data labelled "sample".
3. No external CSS, JS, fonts, images, or icon libraries.
4. No colour gradients, glows, neon, glassmorphism, or large corner radii.
5. No autoplaying animation and no auto-advance.
6. No version numbers, change logs, or status labels in body copy.

## Acceptance

- [ ] Opens offline with zero requests in the network panel
- [ ] All red / white / black / grey values come from the table above
- [ ] All numbers use tabular alignment
- [ ] Keyboard navigation reaches every slide; `Esc` overview works
- [ ] No transitions under `prefers-reduced-motion`
- [ ] Print preview is not clipped and loses no pages; the brand mark appears correctly on every page
- [ ] No real business data; the brand mark appears only in the agreed deck position, undistorted and within limits
- [ ] The mark is either the redraw or an official replacement, and the README trademark notice is intact
