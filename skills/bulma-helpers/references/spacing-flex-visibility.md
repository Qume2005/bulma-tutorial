# Bulma Helper Catalog: Spacing, Flexbox, Visibility & Skeletons

> Full catalogs for spacing, flexbox, display/visibility, float/overflow/other, and skeletons. Bulma v1.0.4. Source of truth: bulma.io (`/documentation/helpers/` and `/documentation/features/skeletons/`).

## Spacing helpers

Source: `/documentation/helpers/spacing-helpers/`.

Bulma ships a numbered spacing scale. Two reset helpers and a full margin/padding scale.

### Reset helpers

```
is-marginless      margin: 0 !important
is-paddingless     padding: 0 !important
```

### The `m*` / `p*` scale

Pattern: `{property}{side}-{size}`.

- `{property}`: `m` (margin) | `p` (padding)
- `{side}`: none (all sides) | `t` (top) | `b` (bottom) | `l` (left) | `r` (right) | `x` (left+right) | `y` (top+bottom)
- `{size}`: `0` `1` `2` `3` `4` `5` `6` `auto` | `n1` `n2` `n3` `n4` `n5` `n6` (negative margins)

So the full surface is:

| Class | Property | Sides |
|-------|----------|-------|
| `m-0` … `m-6`, `m-auto` | margin | all |
| `mt-0` … `mt-6`, `mt-auto` | margin-top | top |
| `mr-0` … `mr-6`, `mr-auto` | margin-right | right |
| `mb-0` … `mb-6`, `mb-auto` | margin-bottom | bottom |
| `ml-0` … `ml-6`, `ml-auto` | margin-left | left |
| `mx-0` … `mx-6`, `mx-auto` | margin-left + margin-right | x-axis |
| `my-0` … `my-6`, `my-auto` | margin-top + margin-bottom | y-axis |
| `p-0` … `p-6` | padding | all |
| `pt-/pr-/pb-/pl-0..6` | padding-{side} | one side |
| `px-0` … `px-6` | padding-left + padding-right | x-axis |
| `py-0` … `py-6` | padding-top + padding-bottom | y-axis |

Negative margins: `mt-n4`, `mx-n2`, etc. (not available on padding).

The numbered sizes map to a fixed `--bulma-*` spacing-variable scale, so `mt-4` is the same `1.5rem` everywhere — no magic numbers.

```html
<div class="mt-4 mb-2 px-5 py-3">Consistent vertical rhythm, horizontal padding</div>
<div class="mx-auto has-text-centered">Centered block (auto x-margins)</div>
<div class="mt-n4">Pulls up into the previous element</div>
```

### Responsive spacing

Append a breakpoint: `{class}-{breakpoint}`.

```
mt-4-mobile          mt-4-touch
mt-4-tablet          mt-4-desktop
mt-4-widescreen      mt-4-fullhd
mt-4-tablet-only     mt-4-desktop-only  ...
```

```html
<div class="px-2 px-4-tablet px-6-desktop">
  Tight padding on phones, more on larger screens.
</div>
```

## Flexbox helpers

Source: `/documentation/helpers/flexbox-helpers/`.

### Display

```
is-flex              display: flex
is-inline-flex       display: inline-flex
is-block             display: block
is-inline            display: inline
is-inline-block      display: inline-block
```

(All of these are also responsive — see Visibility section for the responsive surface.)

### Direction

```
is-flex-direction-row
is-flex-direction-row-reverse
is-flex-direction-column
is-flex-direction-column-reverse
```

### Justify content / Align items / Align content / Wrap

```
is-justify-content-flex-start     is-justify-content-space-between
is-justify-content-center         is-justify-content-space-around
is-justify-content-flex-end       is-justify-content-space-evenly
is-justify-content-left           is-justify-content-right

is-align-items-flex-start   is-align-items-center   is-align-items-flex-end
is-align-items-stretch      is-align-items-baseline

is-flex-wrap-wrap        is-flex-wrap-wrap-reverse   is-flex-wrap-nowrap

is-align-content-flex-start ... is-align-content-center ... is-align-content-flex-end ...
```

### Flex-grow / shrink / basis (per-item)

```
is-flex-grow-0  is-flex-grow-1  is-flex-grow-2  is-flex-grow-3  is-flex-grow-4  is-flex-grow-5
is-flex-shrink-0  is-flex-shrink-1  is-flex-shrink-2  is-flex-shrink-3  is-flex-shrink-4  is-flex-shrink-5
```

```html
<div class="is-flex is-justify-content-space-between is-align-items-center">
  <span>Left</span>
  <span class="is-flex-grow-1 has-text-centered">Grows to fill</span>
  <span>Right</span>
</div>
```

## Display & Visibility helpers (responsive)

Source: `/documentation/helpers/visibility-helpers/`.

Every display helper has responsive variants by appending the breakpoint. The same applies to "hide".

### Show as a display type from a breakpoint

```
is-flex-mobile          is-flex-touch          is-flex-tablet
is-flex-desktop         is-flex-widescreen     is-flex-fullhd
is-flex-tablet-only     is-flex-desktop-only   ...
```

(Replace `flex` with `block`, `inline`, `inline-block`, `inline-flex` for the other display types.)

### Hide at a breakpoint — `is-hidden*`

| Class | Hides when |
|-------|------------|
| `is-hidden` | Always (force-hide) |
| `is-hidden-mobile` | ≤ 768px (mobile) |
| `is-hidden-tablet` | ≥ 769px (tablet and up) |
| `is-hidden-tablet-only` | 769px–1023px |
| `is-hidden-touch` | < 1024px (mobile + tablet) |
| `is-hidden-desktop` | ≥ 1024px (desktop and up) |
| `is-hidden-desktop-only` | 1024px–1215px |
| `is-hidden-widescreen` | ≥ 1216px |
| `is-hidden-widescreen-only` | 1216px–1407px |
| `is-hidden-fullhd` | ≥ 1408px |

```html
<span class="is-hidden-tablet">Only shows on phones</span>
<span class="is-hidden-mobile">Hidden on phones</span>
```

## Other helpers (float, clear, overflow, interaction)

Source: `/documentation/helpers/other-helpers/`.

```
is-pulled-left        float: left
is-pulled-right       float: right
is-clearfix           clearfix hack (contain floats)

is-clipped            overflow: hidden !important
is-relative           position: relative
is-overlay            absolute inset:0 (overlay layer)

is-unselectable       user-select: none
is-clickable          cursor: pointer (non-interactive elements that act as buttons)

is-radiusless         border-radius: 0 !important
is-shadowless         box-shadow: none !important
```

```html
<span class="is-pulled-right is-clickable" onclick="…">Floating action</span>
<div class="is-overlay has-background-black-ter">Full-cover overlay</div>
```

## Skeletons (v1 feature — owned by this skill)

Source: `/documentation/features/skeletons/`.

Skeletons are loading placeholders. Bulma v1 ships **two ways** to use them:

1. **Modifier on any element/component** — add `is-skeleton` (or `has-skeleton`) to turn an existing element into a pulsing skeleton placeholder. Most elements and components support it.

   ```html
   <p class="title is-skeleton">Will appear as an animated bar while loading</p>
   <button class="button is-skeleton">Loading</button>
   ```

2. **Pre-built skeleton utility elements** — dedicated skeleton elements for cases where you don't have a real element yet (e.g. a placeholder card before data arrives). Use these as standalone building blocks.

   ```html
   <div class="skeleton-block"></div>
   <div class="skeleton-lines"><div></div><div></div></div>
   ```

Skeletons are themed (they use `--bulma-skeleton-*` variables) and animate automatically — no JS or custom keyframes needed.

> **Ownership note.** Skeletons are owned here, NOT in `bulma-elements`. The `is-skeleton` / `has-skeleton` *modifiers* and the pre-built skeleton *elements* are utility/helper constructs.

## Quick composition example

```html
<!-- Responsive: stacked on mobile, flex row from tablet up; padding scales with breakpoint -->
<nav class="is-flex is-flex-direction-column is-justify-content-center
           is-flex-direction-row-tablet is-align-items-center-tablet
           px-3 py-4-tablet mb-5">
  <span class="is-hidden-mobile">Logo</span>
  <span class="is-skeleton">Nav item loading…</span>
  <a class="is-pulled-right has-text-link is-clickable">Action</a>
</nav>
```

## References

- Bulma helpers index: https://bulma.io/documentation/helpers/
- Spacing helpers: https://bulma.io/documentation/helpers/spacing-helpers/
- Flexbox helpers: https://bulma.io/documentation/helpers/flexbox-helpers/
- Visibility helpers: https://bulma.io/documentation/helpers/visibility-helpers/
- Other helpers: https://bulma.io/documentation/helpers/other-helpers/
- Skeletons (v1 feature): https://bulma.io/documentation/features/skeletons/
