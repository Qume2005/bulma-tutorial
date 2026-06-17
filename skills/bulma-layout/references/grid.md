# CSS Grid Reference (NEW in Bulma 1.0)

> Part of the `bulma-layout` skill. This file is the full catalog for Bulma's **CSS Grid** system (`grid` / `cell` / `fixed-grid`, plus the Smart Grid which is the default behavior of `grid`), introduced in Bulma 1.0. For the Flexbox `columns` system, see `columns.md`. The two are distinct — do not conflate.

**Bulma doc source:** https://bulma.io/documentation/grid/ (subpages: `grid-cells/`, `smart-grid/`, `fixed-grid/`)

> **v1 note:** This Grid system did NOT exist in Bulma 0.9. It is new in 1.0. Older tutorials and the STALE context7 snapshot (0.9.3) will not mention it. Source everything below from bulma.io.

## How the Grid system works

There are two wrapper roles and one container role:

1. **`grid`** — the actual CSS Grid container (`display: grid`). Direct children become grid items. This is ALSO the Smart Grid: a bare `<div class="grid">` already auto-fits as many columns as possible given a minimum column width (see *Smart Grid* below). There is **no separate `smart-grid` wrapper class** — the smart behavior is the *default* of `grid`.
2. **`fixed-grid`** — a wrapper around a `grid` that fixes the **number of columns** (count is fixed, not auto-fit).

And one child role:

- **`cell`** — a grid item inside a `grid`.

The minimal template is a `grid` (optionally wrapped in `fixed-grid` when you want a fixed column count), with `cell` children:

```html
<div class="fixed-grid">
  <div class="grid">
    <div class="cell">Cell 1</div>
    <div class="cell">Cell 2</div>
    <div class="cell">Cell 3</div>
  </div>
</div>
```

## Fixed Grid (`fixed-grid`)

Use when you want a **fixed number of columns** (a count you control, not auto-fit). The column count is set on the `fixed-grid` wrapper via `has-N-cols`, or made automatic with `has-auto-count`.

> **Do not confuse this with the Smart Grid's `is-col-min-*`** (below). `has-N-cols` sets a *column COUNT* on a `fixed-grid`; `is-col-min-*` sets a minimum column *WIDTH* on a `grid`. Different API, different wrapper, different meaning. This conflation is the #1 Grid mistake.

### `has-auto-count` (the reflow workhorse)

```html
<div class="fixed-grid has-auto-count">
  <div class="grid">
    <div class="cell">1</div>
    <div class="cell">2</div>
    <!-- ... -->
  </div>
</div>
```

`has-auto-count` sets the column count per breakpoint automatically:

| Breakpoint | Columns |
|---|---|
| mobile | 2 |
| tablet | 4 |
| desktop | 8 |
| widescreen | 12 |
| fullhd | 16 |

This is the canonical way to get responsive breakpoint reflow **without** writing your own media queries — ideal for dashboards, tile galleries, admin panels.

### Explicit column count (`has-N-cols`)

You can also pin a specific count (1–12) on the `fixed-grid` with `has-N-cols`:

- `has-2-cols`, `has-3-cols`, `has-4-cols`, … `has-12-cols` — fixed column count.
- With a breakpoint suffix: `has-4-cols-desktop`, `has-6-cols-widescreen`, `has-8-cols-fullhd`, etc.

```html
<div class="fixed-grid has-4-cols">
  <div class="grid"><div class="cell">…</div>…</div>
</div>
```

## Smart Grid (the default `grid` behavior)

> **There is no `smart-grid` wrapper class.** The Smart Grid IS the default `grid`: a bare `<div class="grid">` already auto-fits as many columns as possible given a minimum column width. Apply `is-col-min-*` directly to the `grid` element.

Use the Smart Grid when you want **as many columns as will fit** given a minimum column width — a true auto-fit layout. Set the minimum column width with `is-col-min-*` directly on the `grid`:

```html
<div class="grid is-col-min-12">
  <div class="cell">Tile</div>
  <!-- Bulma fits as many >= 12-unit-wide columns as the viewport allows -->
</div>
```

- `is-col-min-N` sets the minimum column width (N is a size unit, values 0–32; `is-col-min-0` means no minimum — columns auto-size freely). Higher N = fewer/wider columns. This is a minimum *width*, NOT a column *count* — do not conflate it with fixed-grid's `has-N-cols`.
- Gap modifiers apply here too (`is-gap-*`, `is-column-gap-*`, `is-row-gap-*`).
- These can take breakpoint suffixes.

Choose the Smart Grid when each cell is a "card of roughly equal importance" and you don't care about an exact column count — you care that cards never get smaller than a readable width.

## Grid cells (`cell` modifiers)

Each `cell` can span columns/rows and be positioned. All span/start modifiers accept **per-breakpoint suffixes** (e.g. `is-col-span-2`, `is-col-span-2-widescreen`).

### Column spanning
| Class | Effect |
|---|---|
| `is-col-span-2` … `is-col-span-12` | Cell spans N columns. |

### Row spanning
| Class | Effect |
|---|---|
| `is-row-span-2` … `is-row-span-12` | Cell spans N rows. |

### Explicit placement

Bulma documents six cell-placement modifiers. `is-*-start-N` places from the **start** edge; `is-*-from-end-N` places counting from the **end** edge (i.e. negative line index). All accept per-breakpoint suffixes.

| Class | Effect |
|---|---|
| `is-col-start-N` | Place the cell at column line N (from the start). |
| `is-col-from-end-N` | Place the cell at column line N counting from the **end** of the grid. |
| `is-row-start-N` | Place the cell at row line N (from the start). |
| `is-row-from-end-N` | Place the cell at row line N counting from the **end** of the grid. |

```html
<div class="fixed-grid has-auto-count">
  <div class="grid">
    <div class="cell is-col-span-2 is-row-span-2">Big tile (2×2)</div>
    <div class="cell">A</div>
    <div class="cell">B</div>
    <div class="cell">C</div>
    <div class="cell">D</div>
  </div>
</div>
```

### Responsive cell spanning

Append a breakpoint to any span/start modifier:

- `is-col-span-2`, `is-col-span-2-mobile`, `is-col-span-2-tablet`, `is-col-span-2-desktop`, `is-col-span-2-widescreen`, `is-col-span-2-fullhd`, plus `*-only` variants.

This lets a tile be 2-wide on desktop but 1-wide on mobile, e.g. `class="cell is-col-span-2-desktop"`.

## Grid vs Flexbox columns — decision guide

| Need | Use |
|---|---|
| Fixed number of equal-width cells, auto-reflow by breakpoint (2/4/8/12/16) | `fixed-grid` + `has-auto-count` + `grid` + `cell` |
| Fixed column count you pin yourself | `fixed-grid` + `has-N-cols` + `grid` + `cell` |
| Auto-fit cards to a minimum width (count varies with viewport) | `grid` + `is-col-min-*` + `cell` (the Smart Grid — no wrapper) |
| Explicit 2D spanning (`is-col-span-*`, `is-row-span-*`) | Grid (`cell` modifiers) — Flexbox columns CANNOT do this |
| Variable-width, content-driven fractional columns (`is-half`, `is-3`) | Flexbox `columns` / `column` |
| Sidebar + main, where one column is fixed and one flexes | Flexbox `columns` (or Grid with `is-col-span-*`) |
| A simple card row that wraps | Either; `columns is-multiline` is more common and simpler |

**Rule of thumb:** if the user says "grid" and means equal tiles / a dashboard / explicit spanning, it's the CSS Grid system. If they mean "put two divs side by side," it's Flexbox `columns`. Never call `columns` a grid — the names are distinct on purpose.

## When NOT to use Grid

- One-off side-by-side pairs (use `columns`).
- Form field rows (use `field`/`control` from `bulma-form`, or `columns`).
- Layouts where each child must size to its own content and the row is fundamentally 1D.

## References
- Grid overview: https://bulma.io/documentation/grid/
- Grid cells: https://bulma.io/documentation/grid/grid-cells/
- Smart Grid: https://bulma.io/documentation/grid/smart-grid/
- Fixed Grid: https://bulma.io/documentation/grid/fixed-grid/
