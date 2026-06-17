---
name: bulma-layout
description: Bulma layout. Use when the user says "bulma columns", "12-column", "responsive columns", "bulma grid", "cell", "smart-grid", "fixed-grid", "container", "level", "media object", "hero", "section", or "footer", or asks how to arrange content side by side, build a responsive multi-column row, or structure a Bulma page. Covers the Flexbox columns system AND the v1 CSS Grid system (grid/cell/smart-grid/fixed-grid) introduced in Bulma 1.0.
---

# Bulma Layout

## When to use this skill

- You need to arrange content side by side: "bulma columns", "12-column", "responsive columns", "arrange side by side".
- You need a real CSS Grid with fixed/equal cells that reflow by breakpoint: "bulma grid", "cell", "smart-grid", "fixed-grid", "is-col-span", "has-auto-count".
- You need to structure a page: `container` (centered max-width), `section`, `footer`, `hero` (full-width banner, fullheight), `level` (horizontal flex rows), `media-object` (avatar + content + right).
- You need to choose between Flexbox columns and CSS Grid for a given layout.

**Out of scope (owned by siblings):**
- Spacing/margin/padding *utility classes* (`m*`, `p*`, `is-flex`) → `bulma-helpers`. (Layout owns the structural elements; helpers own the utilities.)
- The `.block` element (v1 vertical-rhythm spacer between stacked elements) → `bulma-elements`.
- Navbar / dropdown / tabs / pagination (navigation components) → `bulma-components`.

## Two distinct systems — read this first

Bulma ships **two** layout systems. They are NOT interchangeable and must not be conflated (this is a v1-accuracy requirement).

| System | Class | Underlying tech | Best for |
|---|---|---|---|
| **Flexbox columns** | `columns` / `column` | Flexbox (1D) | Content-driven rows where each column sizes itself or takes a fractional width (`is-half`, `is-one-third`). Variable-width children. |
| **CSS Grid** (NEW in v1) | `grid` / `cell` | CSS Grid (2D) | Fixed/equal-cell layouts that reflow cleanly by breakpoint, with explicit column/row spanning. Dashboards, galleries, tile grids. |

**Decision rule (one line):** If you want a fixed number of equal-width cells that reflow by breakpoint and/or need 2D row+column spanning → use **Grid** (`fixed-grid` + `has-N-cols`/`has-auto-count`, or a bare `grid` for auto-fit). If you want a flexible row of variable-width content columns that size by fraction (`is-half`, `is-3`) → use **columns**.

## Key classes & concepts

**Flexbox columns (unchanged since pre-v1):**
- `columns` — the flex container; `column` — each child.
- Fractional sizes on the child: `is-three-quarters`, `is-two-thirds`, `is-half`, `is-one-third`, `is-one-quarter`, `is-full`, and 12-step `is-1`…`is-12`.
- Row behavior: `is-multiline` (wrap children onto new lines), `is-gapless` (remove gaps), `is-variable is-N` (custom gap, 0–8), `is-centered`, `is-vcentered`.
- Offset: `is-half-offset`, `is-one-third-offset`, and `is-N-offset` (0–11).
- Responsiveness on columns applies by breakpoint (`is-3-mobile`, `is-half-desktop`, etc.).

**CSS Grid (NEW in v1, lives at `/documentation/grid/`):**
- `grid` — the 2D grid container; `cell` — each grid child.
- `fixed-grid` — wrap a `grid` to lock a *fixed* column count; set the count with `has-N-cols` (1–12) or auto-set it per breakpoint with `has-auto-count` (2 mobile / 4 tablet / 8 desktop / 12 widescreen / 16 fullhd).
- Smart Grid — the default `grid` itself auto-fits as many columns as possible given a minimum column width; set it with `is-col-min-*` (0–32) directly on the `grid`. NOTE: `has-N-cols` sets a column COUNT (on `fixed-grid`); `is-col-min-*` sets a column WIDTH (on `grid`). Two distinct APIs — do not conflate.
- Cell spanning & placement (per breakpoint): `is-col-span-*`, `is-row-span-*`, `is-col-start-*` / `is-row-start-*`, and `is-col-from-end-*` / `is-row-from-end-*`.

**Page structure (layout/* elements):**
- `container` — centered max-width wrapper (default ~1024px); `is-widescreen` (up to 1216px), `is-fullhd` (up to 1408px), `is-max-desktop`, `is-max-widescreen`.
- `section` — vertical padding band; `is-medium`, `is-large` increase it.
- `footer` — page footer band (typically dark, multi-column via `columns` inside).
- `hero` — full-width banner; sections `hero-head`, `hero-body`, `hero-foot`; sizes `is-medium`, `is-large`, `is-fullheight`, `is-fullheight-with-navbar`; colors via `is-primary`/`is-link`/etc.
- `level` — horizontal flex row of left/center/right items; `level-item`, `level-left`, `level-right`, `is-mobile` (keeps horizontal on mobile).
- `media-object` — avatar + content + optional right element; `media`, `media-left`, `media-content`, `media-right`.

## Common patterns (copy-paste)

### 1. Flexbox row of variable-width columns

```html
<div class="columns">
  <div class="column is-two-thirds">Main content</div>
  <div class="column">Sidebar (fills the rest)</div>
</div>
```

### 2. Responsive wrapping columns (cards grid, content-driven)

```html
<div class="columns is-multiline is-variable is-4">
  <div class="column is-one-third-desktop is-half-mobile">Card A</div>
  <div class="column is-one-third-desktop is-half-mobile">Card B</div>
  <div class="column is-one-third-desktop is-half-mobile">Card C</div>
</div>
```

### 3. Fixed CSS Grid that reflows by breakpoint (the dashboard case)

```html
<div class="fixed-grid has-auto-count">
  <div class="grid">
    <div class="cell">1</div>
    <div class="cell">2</div>
    <div class="cell is-col-span-2">spans 2 cols</div>
    <div class="cell">4</div>
  </div>
</div>
<!-- has-auto-count: 2 cols on mobile, 4 tablet, 8 desktop, 12 widescreen, 16 fullhd -->
```

### 4. Smart Grid (auto-fit to a minimum column width)

```html
<div class="grid is-col-min-12">
  <div class="cell">Tile</div>
  <!-- Bulma fits as many >= 12-unit-wide columns as the viewport allows -->
</div>
```

### 5. Page skeleton (container + section + hero + footer)

```html
<section class="hero is-medium is-primary">
  <div class="hero-body">
    <div class="container"><p class="title">Page hero</p></div>
  </div>
</section>

<section class="section">
  <div class="container"><!-- main content (columns or grid here) --></div>
</section>

<footer class="footer">
  <div class="container"><p>Footer</p></div>
</footer>
```

### 6. Level (horizontal row) and media object

```html
<div class="level is-mobile">
  <div class="level-left"><div class="level-item"><strong>Left</strong></div></div>
  <div class="level-right"><div class="level-item">Right</div></div>
</div>

<article class="media">
  <figure class="media-left"><p class="image is-64x64"><img src="avatar.png"></p></figure>
  <div class="media-content">Content here</div>
</article>
```

## v1 gotchas

- **CSS Grid is NEW in v1.** It did not exist in Bulma 0.9. A model with only pre-v1 knowledge will tell you "Bulma has no grid" — that is now false. The Grid docs live at `https://bulma.io/documentation/grid/` (overview, `grid-cells/`, `smart-grid/`, `fixed-grid/`).
- **Grid and Flexbox `columns` coexist.** They are independent systems with independent class names (`grid`/`cell` vs `columns`/`column`). Do not mix them on the same element and do not call `columns` a "grid."
- **Choose by intent, not familiarity.** Grid for fixed/equal-cell 2D layouts with spanning; `columns` for content-driven fractional rows. The baseline failure this skill fixes is forcing a fixed-cell dashboard through the Flexbox `columns` API (see `baseline-failure.md`).
- **`fixed-grid` + `has-auto-count`** is the canonical way to get automatic breakpoint reflow (2/4/8/12/16) without media queries. Pin a specific count yourself with `has-N-cols` (1–12).
- **`hero is-fullheight-with-navbar`** accounts for an externally fixed navbar; plain `is-fullheight` assumes no fixed navbar.
- The contract's guessed Grid URL (`features/grid-system/`) is wrong — use `/documentation/grid/`.

## References (load on demand)

Load these bundled files when you need full catalogs, modifier tables, or richer examples. Every file below is part of this skill:

- `references/columns.md` — Full Flexbox `columns`/`column` catalog: fractional + 12-step sizes, offsets, `is-multiline`/`is-gapless`/`is-variable`/gap sizes, vertical/centered alignment, nesting, and per-breakpoint responsive sizing.
- `references/grid.md` — Full v1 CSS Grid catalog: `grid`/`cell`, `fixed-grid` (`has-auto-count` → 2/4/8/12/16, explicit `has-N-cols`), Smart Grid (the default `grid` with `is-col-min-*`), cell spanning & placement (`is-col-span-*`/`is-row-span-*`/`is-col-start-*`/`is-row-start-*`/`is-col-from-end-*`/`is-row-from-end-*`) per breakpoint, and a Grid-vs-Flexbox decision guide.
- `references/page-structure.md` — `container` (max-widths and `is-widescreen`/`is-fullhd`/`is-max-*`), `section` (sizes), `footer`, `hero` (sizes, colors, `hero-head`/`hero-body`/`hero-foot`, `is-fullheight` variants), `level` (`level-item`/`level-left`/`level-right`, `is-mobile`), `media-object` (`media-left`/`media-content`/`media-right`).
