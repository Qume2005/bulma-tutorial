# Bulma Helper Catalog: Color & Typography

> Full catalogs for color helpers and typography helpers. Bulma v1.0.4. Source of truth: bulma.io (`/documentation/helpers/`).

## The 8 theme colors (and only these)

Color helpers apply to Bulma's **8 theme colors**. This is the authoritative list — there are NOT 9.

| Theme color | Derived from (Sass) | Semantic use |
|-------------|---------------------|--------------|
| `primary` | `$turquoise` | Brand / default action color |
| `link` | `$blue` | Links, `is-link` buttons |
| `info` | `$cyan` | Informational accents |
| `success` | `$green` | Success states |
| `warning` | `$yellow` | Warning states |
| `danger` | `$red` | Destructive / error states |
| `light` | (`$white-ter`-family) | Light surfaces |
| `dark` | (`$grey-dark`-family) | Dark surfaces |

**NOT theme colors** (do not list these as theme colors):
- `white` and `black` are *base* colors (`$white`, `$black` in `initial-variables`).
- `grey` (and `grey-light`, `grey-dark`, `grey-lighter`, `grey-darker`, `black-bis`, `black-ter`, `white-bis`, `white-ter`) live in the **`$shades`** map. They are shades, not theme colors. They DO have `has-text-*` / `has-background-*` helpers, but they are not part of the 8-color theme set.

In v1, each theme color emits a set of `--bulma-*` CSS variables (`--bulma-primary`, `--bulma-primary-light`, `--bulma-primary-dark`, `--bulma-primary-invert`, `--bulma-primary-hsl`, etc.). The helper classes resolve to these variables, so they automatically track theme/dark-mode overrides. (Defining new colors or overriding these variables → `bulma-customize`.)

## Color helpers

### Text color — `has-text-*`

Sets `color`. Works on any element.

```
has-text-{color}
has-text-{color}-light     (lighter variant)
has-text-{color}-dark      (darker variant)
```

Where `{color}` is any of: the 8 theme colors, plus `white`, `black`, and the shades (`black-bis`, `black-ter`, `grey-darker`, `grey-dark`, `grey`, `grey-light`, `grey-lighter`, `white-ter`, `white-bis`).

```html
<p class="has-text-primary">Primary text</p>
<p class="has-text-link">Link-colored text</p>
<p class="has-text-grey-light">Muted text</p>
<p class="has-text-danger-dark">Darker danger text</p>
```

### Background color — `has-background-*`

Sets `background-color`. Same `{color}` surface as `has-text-*`, including `-light` / `-dark` variants.

```html
<div class="has-background-info-light p-4">Soft info background</div>
<div class="has-background-danger">Danger background</div>
<div class="has-background-black-ter">Dark grey background</div>
```

### Responsive color (v1)

Color helpers accept breakpoint suffixes (mobile/tablet/desktop/widescreen/fullhd and their `-only` variants):

```html
<p class="has-text-centered-mobile has-text-left-tablet">
  Centered on mobile, left-aligned from tablet up.
</p>
```

## `palette-*` helper CLASSES (owned by this skill)

Bulma v1 generates a **palette** of in-between shades for each theme color (21 shades per color in 5% lightness increments). The **helper classes** that apply a palette shade to an element are:

- `has-text-{color}-{shade}` — e.g. `has-text-primary-05`, `has-text-primary-55`, `has-text-primary-95` (shade = `00`–`100` in steps of 5).
- `has-background-{color}-{shade}` — same pattern for background.

```html
<p class="has-text-primary-55">A mid-shade primary</p>
<div class="has-background-link-10 px-3 py-2">Very light link tint</div>
```

> **Ownership boundary.** This skill owns the `palette-*` helper CLASSES (applying an existing shade). The palette *concept* — what palettes are, how the 21-shade generation works, the underlying `--bulma-{color}-*` variable structure, and how to define/register a custom palette — is owned by **`bulma-customize`**. For theming, dark mode, or new color definitions, route to `bulma-customize`.

## Typography helpers

Source: `/documentation/helpers/typography-helpers/`.

### Size — `is-size-1` .. `is-size-7`

7 sizes; `is-size-1` is largest, `is-size-7` is smallest. Applies to any element (not just `title`).

```html
<p class="is-size-3">Heading-sized paragraph</p>
```

Responsive variants:

```
is-size-1-mobile          is-size-1-touch
is-size-1-tablet          is-size-1-desktop
is-size-1-widescreen      is-size-1-fullhd
is-size-1-tablet-only     is-size-1-desktop-only  ...
```

### Weight — `has-text-weight-*`

```
has-text-weight-light       (300)
has-text-weight-normal      (400)
has-text-weight-medium      (500)
has-text-weight-semibold    (600)
has-text-weight-bold        (700)
has-text-weight-extrabold   (800, added in v1)
```

```html
<span class="has-text-weight-semibold">Semibold</span>
```

### Alignment — `has-text-*`

```
has-text-centered
has-text-justified
has-text-left
has-text-right
```

Responsive: append the breakpoint (`has-text-left-mobile`, `has-text-centered-tablet-only`, …).

### Text transformation — `is-*`

```
is-capitalized
is-lowercase
is-uppercase
is-italic
is-underlined
```

### Font family — `is-family-*`

```
is-family-primary
is-family-secondary
is-family-sans-serif
is-family-monospace
is-family-code
is-family-custom
```

```html
<code class="is-family-code">monospace-ish code</code>
```

## Quick composition example

```html
<p class="has-text-primary has-text-weight-bold is-size-4 has-text-centered is-uppercase">
  Themed, weighted, sized, centered, uppercased
</p>
<div class="has-background-info-light is-size-7 p-3">
  Soft tinted callout, small text, padded with the spacing scale (p-3 — see spacing-flex-visibility.md).
</div>
```

## References

- Bulma helpers index: https://bulma.io/documentation/helpers/
- Color helpers: https://bulma.io/documentation/helpers/color-helpers/
- Typography helpers: https://bulma.io/documentation/helpers/typography-helpers/
- Sass source (theme color derivation): https://github.com/jgthms/bulma/blob/main/sass/utilities/derived-variables.scss
- Palette CONCEPT (NOT owned here): see `bulma-customize` → `references/css-variables.md`
