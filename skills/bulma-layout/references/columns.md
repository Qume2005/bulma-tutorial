# Flexbox Columns Reference

> Part of the `bulma-layout` skill. This file is the full catalog for Bulma's **Flexbox columns** system (`columns` / `column`). For the v1 CSS Grid system, see `grid.md`. The two are distinct.

**Bulma doc source:** https://bulma.io/documentation/columns/ (subpages: `basics/`, `sizes/`, `gap/`, `options/`, `responsiveness/`, `nesting/`)

## How the system works

- The container `.columns` is `display: flex; flex-wrap: wrap` (when `is-multiline`).
- Each direct child `.column` is a flex item with `flex: 1` by default — so equal children share the row evenly.
- Sizing a child with `is-N` overrides the default flex-grow to a fractional width (out of 12).

```html
<div class="columns">
  <div class="column">Equal</div>
  <div class="column">Equal</div>
  <div class="column">Equal</div>
</div>
```

## Column sizes (on the `.column` child)

### Fractional sizes
| Class | Width |
|---|---|
| `is-three-quarters` | 75% |
| `is-two-thirds` | ~66.6% |
| `is-half` | 50% |
| `is-one-third` | ~33.3% |
| `is-one-quarter` | 25% |
| `is-full` | 100% (own row) |

### 12-step sizes
`is-1` … `is-12` — each is N/12 of the row.

```html
<div class="columns">
  <div class="column is-2">2/12</div>
  <div class="column is-8">8/12</div>
  <div class="column is-2">2/12</div>
</div>
```

### Offsets (push a column right)
- `is-offset-half`, `is-offset-one-third`, `is-offset-one-quarter`, `is-offset-three-quarters`, `is-offset-two-thirds`.
- `is-offset-1` … `is-offset-11` (12-step).

```html
<div class="columns">
  <div class="column is-4 is-offset-8">Right-aligned quarter-ish</div>
</div>
```

## Container modifiers (on `.columns`)

| Modifier | Effect |
|---|---|
| `is-multiline` | Allow children to wrap onto new rows (children's widths sum > 100% then wrap). |
| `is-gapless` | Remove the inter-column gap. |
| `is-variable is-N` | Custom gap size, N = 0…8 (e.g. `is-variable is-3`). Default gap is `0.75rem` (`$column-gap`). |
| `is-centered` | Horizontally center the columns. |
| `is-vcentered` | Vertically center the columns in the row. |
| `is-mobile` | Keep the flex layout horizontal even on mobile (columns stack vertically by default on mobile). |
| `is-desktop` | Only apply the columns layout from `desktop` up; stack on mobile/tablet. |

## Gap sizes (`is-variable is-N`)
The gap appears on both sides of each column, so the visible gap between two adjacent columns is `2 * gap`.

| `is-N` | Gap per side |
|---|---|
| `is-0` | 0 |
| `is-1` | 0.25rem |
| `is-2` | 0.5rem |
| `is-3` | 0.75rem (default) |
| `is-4` | 1rem |
| `is-5` | 1.25rem |
| `is-6` | 1.5rem |
| `is-7` | 1.75rem |
| `is-8` | 2rem |

## Vertical alignment
- On the container: `is-centered` (horizontal), `is-vcentered` (vertical).
- On a child `.column`: `is-narrow` makes the column take only its content width instead of `flex: 1`.

## Responsiveness (per breakpoint)

Size/offset classes accept a breakpoint suffix. Bulma's breakpoints:

| Name | From |
|---|---|
| `mobile` | 0 (only, via `*-mobile` when combined with `is-mobile`) |
| `tablet` | ≥ 769px |
| `desktop` | ≥ 1024px |
| `widescreen` | ≥ 1216px |
| `fullhd` | ≥ 1408px |

Suffix forms: `is-N` (applies at `tablet` and up by default), `is-N-mobile`, `is-N-tablet`, `is-N-desktop`, `is-N-widescreen`, `is-N-fullhd`, plus `*-only` variants (`is-N-tablet-only`, `is-N-desktop-only`, etc.).

```html
<div class="columns is-multiline">
  <!-- full width on mobile, half on tablet+, third on desktop+ -->
  <div class="column is-full is-half-tablet is-one-third-desktop">Card</div>
  <div class="column is-full is-half-tablet is-one-third-desktop">Card</div>
  <div class="column is-full is-half-tablet is-one-third-desktop">Card</div>
</div>
```

## Nesting

You can nest `columns` inside a `column`. The nested `columns` must be the direct child for the gap/margin math to work.

```html
<div class="columns">
  <div class="column is-half">
    <div class="columns is-gapless">
      <div class="column">A</div>
      <div class="column">B</div>
    </div>
  </div>
  <div class="column is-half">Right</div>
</div>
```

## When to use Flexbox columns (not Grid)

- Children are content-driven and variable width.
- You want fractional sizing (`is-half`, `is-3`) rather than fixed equal cells.
- You don't need explicit 2D row+column spanning.
- You're laying out a sidebar/main pair, a card row that wraps, or form fields in a row.

If you instead need a fixed number of equal-width cells that reflow by breakpoint, or explicit spanning (`is-col-span-*` / `is-row-span-*`), use the CSS Grid system — see `grid.md`.

## References
- Columns basics: https://bulma.io/documentation/columns/basics/
- Column sizes: https://bulma.io/documentation/columns/sizes/
- Columns gap: https://bulma.io/documentation/columns/gap/
- Column options: https://bulma.io/documentation/columns/options/
- Column responsiveness: https://bulma.io/documentation/columns/responsiveness/
- Column nesting: https://bulma.io/documentation/columns/nesting/
