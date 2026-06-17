# Reference: Bulma Modifiers & The 8 Theme Colors

> Bundled with `bulma-fundamentals`. Covers the modifier *ecosystem/grammar* and the 8 theme-color palette as a concept. The full helper utility-class catalog (`is-size-*`, `has-text-*`, `has-text-weight-*`, spacing `m*`/`p*`, `is-hidden-*`, skeletons) belongs to `bulma-helpers`; deep theming/override recipes belong to `bulma-customize`.

## 1. The modifier grammar

Every Bulma element/component is a base class, and you change its appearance by appending **modifier classes**. All modifiers are prefixed `is-` or `has-`. There are NO bare-word classes — `class="button primary"` is unstyled; it must be `class="button is-primary"`.

```
<element class="BASE  is-MODIFIER  has-MODIFIER">
```

- `is-*` — most common prefix: state (`is-active`, `is-loading`), size (`is-small`, `is-medium`, `is-large`), color (`is-primary`, `is-info`), style (`is-outlined`, `is-rounded`).
- `has-*` — applies a property "to" the element, often layout/coupling: `has-addons`, `has-text-centered`, `has-background-*`. (Color/typography/spacing helpers using `has-*` / `is-size-*` are owned by `bulma-helpers`.)

## 2. Modifier categories (applies across elements that support them)

| Category | Examples | Notes |
|---|---|---|
| **Color** | `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`, `is-light`, `is-dark` | The 8 theme colors (see §3). Available on buttons, tags, notifications, messages, progress, etc. |
| **Size** | `is-small`, `is-normal`, `is-medium`, `is-large` | Element-relative. (`is-size-1..7` typography sizes are helpers → `bulma-helpers`.) |
| **State** | `is-active`, `is-hovered`, `is-focused`, `is-loading`, `is-static`, `is-disabled` | `is-active` is the universal toggle for navbar/dropdown/modal/tabs (JS-owned in `bulma-components`). |
| **Style** | `is-outlined`, `is-inverted`, `is-rounded`, `is-fullwidth`, `is-narrow`, `is-bordered`, `is-striped` | Style-flavor modifiers; which ones exist depends on the element. |
| **Light/Dark variant** | `is-primary is-light`, `is-danger is-dark` | Combines a color with a light/dark tint for subtle backgrounds. |

The full list of which modifier applies to which element lives in that element's skill (`bulma-elements`, `bulma-components`, `bulma-form`). This table is the grammar, not an element-by-element catalog.

## 3. The 8 theme colors

These are the colors you pass to `is-<color>` (and to the `has-text-<color>` / `has-background-<color>` helpers). Bulma defines them as derived semantic names that map onto a base palette:

| Theme color (`is-<color>`) | Base variable it derives from |
|---|---|
| `primary` | `$turquoise` |
| `link` | `$blue` |
| `info` | `$cyan` |
| `success` | `$green` |
| `warning` | `$yellow` |
| `danger` | `$red` |
| `light` | `$light` |
| `dark` | `$dark` |

(Plus `white` / `black` as base values, used internally for contrast. `grey` is NOT a theme color — it lives in the `$shades` map, so there is no `is-grey` modifier. `has-text-grey-*` shade classes are owned by `bulma-helpers`.)

> **Exact color values are intentionally not listed here.** Bulma defines each color as a Sass `$variable` (in `sass/utilities/initial-variables.sass`) and exposes the rendered value as a `--bulma-*` CSS custom property at runtime. Note: in v1 the compiled `--bulma-*` values are re-derived at compile time and can DIFFER from the raw Sass `$variable` values. For the precise values and how to override them, see the `bulma-customize` skill.

> The 8-color concept stops here. To **define new colors**, **override** `--bulma-primary`, register **color palettes**, or build **custom themes/dark mode**, use `bulma-customize`. To apply color as **text/background utility classes** on arbitrary elements (`has-text-primary`, `has-background-danger`), use `bulma-helpers`.

### Light / dark color variants

Any theme color can be combined with `is-light` or `is-dark` for a tinted background:

```html
<div class="notification is-primary is-light">Soft teal background</div>
<div class="message is-danger is-dark">Deep red background</div>
```

In v1 each color also generates a full **palette** of shades (`--bulma-primary-00` … `--bulma-primary-100`, plus `*-light`/`*-invert`/`*-on-sun`/`*-on-sun-light` etc.) as CSS variables. The palette *concept* and registration is owned by `bulma-customize`.

## 4. v1 note: colors are CSS variables

In Bulma 1.0, each color is exposed as a `--bulma-*` CSS variable (e.g. `--bulma-primary`, `--bulma-link`). Sass variables like `$primary` compile **down** to these CSS variables at build time. This is why:

- overriding a color at runtime = overriding the CSS variable (no rebuild needed), and
- hardcoding hex into a custom class bypasses the system and breaks dark mode / palette generation.

The actionable override recipe (where to put the `:root` rule, how dark mode flips them, how palettes are registered) is in `bulma-customize`.

## Sources
- https://bulma.io/documentation/start/overview/ — syntax & modifier overview, color system concept
- https://bulma.io/documentation/helpers/ — the `is-*`/`has-*` modifier ecosystem
- https://bulma.io/documentation/features/css-variables/ — `--bulma-*` as primary theming in v1; Sass compiles to CSS variables
- https://bulma.io/documentation/features/color-palettes/ — auto-generated palette variables per color
- https://bulma.io/documentation/customize/list-of-sass-variables/ — base (`$turquoise`, `$blue`, …) and derived (`$primary`, `$link`, …) Sass variable definitions
