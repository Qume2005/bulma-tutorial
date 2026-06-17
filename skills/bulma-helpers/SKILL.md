---
name: bulma-helpers
description: Bulma helper utility classes. Use when the user asks about color helpers (has-text-*, has-background-*), typography helpers (has-text-weight-bold, is-size-*), spacing helpers (m*, p*, mx/my/px/py), flexbox helpers (is-flex, is-inline-flex, flex-direction/gap helpers), visibility and responsive display helpers (is-hidden-mobile, is-hidden-tablet-only, is-block/is-inline), float/clear/overflow helpers, or skeleton loaders (bulma-skeleton). Helpers are tiny utility classes you can apply to any element.
---

# Bulma Helpers (utility classes)

## When to use this skill

- "how do I color text/background in Bulma" — `has-text-*`, `has-background-*`
- "Bulma spacing / margin / padding helpers" — `m*`/`p*` scale, `mt-4`, `px-5`, `is-marginless`
- "Bulma typography helpers" — `is-size-*`, `has-text-weight-*`, `has-text-centered`, `is-uppercase`, `is-family-*`
- "hide/show element on mobile/tablet" — `is-hidden-mobile`, `is-block-tablet`, display helpers
- "Bulma flexbox helpers" — `is-flex`, `is-flex-direction-column`, `is-justify-content-*`
- "Bulma skeleton / loading placeholder" — `is-skeleton`, `has-skeleton`, skeleton utility elements
- "apply a palette shade" — `has-text-{color}-{shade}`, `palette-*` helper classes

**Out of scope — route elsewhere:**
- Defining new colors, overriding `--bulma-*` variables, the palette *concept*/variable *definitions*, dark mode, themes, Sass color variables → **`bulma-customize`**. This skill owns the `palette-*` *helper classes* (applying an existing shade) only.
- Element-specific classes (`notification`, `button`, `title`…) → **`bulma-elements`**.
- The `.block` vertical-rhythm element → **`bulma-elements`** (it is an element, not a spacing helper).
- Form field states (`is-success`/`is-danger` with `help` text) → **`bulma-form`**.

## Key classes & concepts

- **8 theme colors** the color helpers apply to: `primary`, `link`, `info`, `success`, `warning`, `danger`, `light`, `dark`. (`white`/`black` are base colors; `grey` and its variants live in the `$shades` map — none of these three are theme colors.)
- **Color helpers**: `has-text-{color}`, `has-background-{color}`, with `-light`/`-dark` variants. Resolve to `--bulma-*` CSS variables, so they track theme/dark-mode overrides automatically.
- **Typography**: `is-size-1..7`, `has-text-weight-{light,normal,medium,semibold,bold,extrabold}`, `has-text-{centered,justified,left,right}`, `is-{uppercase,lowercase,capitalized,italic}`, `is-family-{primary,secondary,sans-serif,monospace,code,custom}`.
- **Spacing scale**: `m*`/`p*` + sides `t/r/b/l/x/y` + sizes `0..6` and `auto` (negative `n1..n6` for margins). E.g. `mt-4`, `px-5`, `py-2`, `mx-auto`. Also `is-marginless` / `is-paddingless`.
- **Flexbox**: `is-flex`, `is-inline-flex`, `is-flex-direction-*`, `is-justify-content-*`, `is-align-items-*`, `is-flex-wrap-*`, `is-flex-grow-*`, `is-flex-shrink-*`.
- **Display & visibility**: `is-block`/`is-inline`/`is-inline-block`/`is-flex`/`is-inline-flex`; `is-hidden`, `is-hidden-mobile`, `is-hidden-tablet`, `is-hidden-touch`, `is-hidden-desktop`, `is-hidden-widescreen`, `is-hidden-fullhd`, plus `-only` variants.
- **Float / overflow / other**: `is-pulled-left`/`-right`, `is-clearfix`, `is-clipped`, `is-relative`, `is-overlay`, `is-unselectable`, `is-clickable`, `is-radiusless`, `is-shadowless`.
- **`palette-*` helper classes** (this skill owns): `has-text-{color}-{shade}`, `has-background-{color}-{shade}` (shade `00`–`100`, step 5). Applies a generated shade; the palette *concept* is `bulma-customize`.
- **Skeletons** (new in v1, owned here): `is-skeleton` / `has-skeleton` modifier on any element/component, plus pre-built skeleton utility elements. Animated, themed placeholders.

## Common patterns (copy-paste)

Color + typography, no inline styles:

```html
<p class="has-text-link has-text-weight-bold is-size-4 has-text-centered">
  Themed, weighted, sized, centered
</p>
<div class="has-background-info-light p-3 is-size-7">Soft tinted callout</div>
```

Spacing scale (no magic numbers), responsive padding:

```html
<section class="px-2 px-4-tablet px-6-desktop py-4 mb-6">
  <div class="mx-auto has-text-centered">Centered, max-width via auto x-margins</div>
</section>
```

Responsive show/hide (declarative, no media queries):

```html
<span class="is-hidden-mobile">Only on phones</span>
<span class="is-hidden-tablet">Only on tablet-and-up</span>
<nav class="is-flex is-flex-direction-column is-flex-direction-row-tablet is-align-items-center-tablet">
  Responsive flex layout
</nav>
```

Skeleton loader (v1 feature):

```html
<p class="title is-skeleton">Title appears as an animated bar while loading</p>
<button class="button is-skeleton">Loading</button>
```

Float / overlay / unselectable:

```html
<a class="is-pulled-right is-clickable">Floating action</a>
<div class="is-overlay has-background-black-ter">Full-cover overlay</div>
```

## v1 gotchas

- **Color helpers resolve to `--bulma-*` CSS variables**, not hard-coded hex. So `has-text-link` automatically tracks theme/dark-mode overrides — never replace a helper with an inline `style="color:#…"`; it silently breaks dark mode. (Deep theming / overriding the variables → `bulma-customize`.)
- **Responsive variants exist on most helpers.** Spacing (`mt-4-tablet`), color (`has-text-centered-mobile`), display (`is-flex-desktop`), and hide (`is-hidden-widescreen-only`) all accept breakpoint suffixes (`mobile`, `tablet`, `tablet-only`, `touch`, `desktop`, `desktop-only`, `widescreen`, `widescreen-only`, `fullhd`).
- **`is-flex-grow-N` / `is-flex-shrink-N`** (N = 0–5) are per-item flex helpers — use them on flex *children*, not the container.
- **Skeletons are a v1 feature owned here** (not in elements). Use `is-skeleton` on an existing element/component, or the pre-built skeleton utility elements. No JS or custom keyframes needed.
- **`palette-*` vs palette concept.** This skill owns the `palette-*` *helper classes* (`has-text-{color}-{shade}`). The palette *concept*, the `--bulma-{color}-*` variable structure, and registering custom palettes → `bulma-customize`.

## References (load on demand)

Load these bundled files for full catalogs, state tables, and richer examples. Every file below is part of this skill:

- `references/color-typography.md` — Full color helper catalog (`has-text-*`, `has-background-*`, light/dark variants, the 8 theme colors + shades), the `palette-*` helper classes, and the full typography helper catalog (sizes, weights, alignment, transformation, font family).
- `references/spacing-flex-visibility.md` — Full spacing scale (`m*`/`p*`, sides, sizes `0..6`/`auto`/negative, responsive), flexbox helpers, display & visibility helpers (`is-hidden*`, responsive display), float/clear/overflow/other helpers, and skeletons (`is-skeleton`, `has-skeleton`, pre-built skeleton elements).
