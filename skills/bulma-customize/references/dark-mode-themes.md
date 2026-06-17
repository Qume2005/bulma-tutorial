# Bulma Dark Mode, Themes & Color Palettes — Reference

> Scope: built-in dark mode, runtime theme switching, custom themes, and color palettes (concept + variables). Sourced from:
> - https://bulma.io/documentation/features/dark-mode/
> - https://bulma.io/documentation/features/themes/
> - https://bulma.io/documentation/features/color-palettes/

## Dark mode is built-in (no plugin)

Bulma 1.0 ships automatic dark mode. It reacts to the OS/browser preference via the `prefers-color-scheme` media query, and you can force a side with theme classes.

### Automatic (follows OS)
```html
<html>
  <!-- no class: Bulma follows prefers-color-scheme -->
  <body>...</body>
</html>
```

### Forced via class
```html
<html class="theme-dark">  <!-- force dark -->
<html class="theme-light"> <!-- force light -->
```

### Toggle from JS
```js
document.documentElement.classList.toggle('theme-dark');
// or swap theme-dark <-> theme-light
```
No rebuild, no framework. Because dark mode is just a batch of `--bulma-*` overrides applied under a selector, toggling the class flips every component instantly.

### How it works under the hood
`.theme-dark` (and the `prefers-color-scheme: dark` block) redefine a set of `--bulma-*` variables — e.g. `--bulma-scheme-main`, `--bulma-scheme-invert`, `--bulma-text`, `--bulma-border`, plus each theme color's derived values. Components read those vars, so they repaint without a rebuild.

## Themes

> "In Bulma, a **theme** is a collection of CSS variables." Bulma ships **2 default themes** (light and dark) because every CSS variable needs a default.

### Defining a custom theme
A custom theme is any selector that overrides a batch of `--bulma-*` variables:

```css
.brand-cyber {
  --bulma-scheme-main: hsl(260, 60%, 8%);
  --bulma-scheme-invert: hsl(0, 0%, 96%);
  --bulma-primary: hsl(280, 90%, 60%);
  --bulma-text: hsl(260, 30%, 90%);
  --bulma-border: hsl(280, 30%, 25%);
  --bulma-link: hsl(190, 90%, 60%);
}
```
Apply it to any container:
```html
<div class="brand-cyber">
  <!-- everything inside uses the cyber theme -->
  <button class="button is-primary">Neon</button>
</div>
```

### Theme vs Sass build
- A theme is RUNTIME (CSS vars under a selector) → switchable live.
- Sass `with (...)` is BUILD-TIME → baked default.
- You can have the Sass build define the "light" default and CSS-var themes provide the alternates.

### Relationship to dark mode
Dark mode IS just a theme (two of them ship by default). Your custom themes coexist with `theme-dark`/`theme-light`; put them on different containers or toggle carefully if they overlap.

## Color palettes (CONCEPT — applying them is in `bulma-helpers`)

> Bulma auto-generates **21 lightness shades** per registered color, from ~0% to 100% in 5% steps. (Source: bulma.io/documentation/features/color-palettes/)

This skill owns the **concept and registration** of palettes. The `palette-*` CLASSES that APPLY a shade to an element are owned by `bulma-helpers` — do not duplicate them here.

### What a palette provides
For a registered color `foo`, Bulma emits CSS variables:
```
--bulma-foo-00  (≈0% lightness)
--bulma-foo-05
--bulma-foo-10
...
--bulma-foo-100 (≈100% lightness)
```
plus `--bulma-foo` (the base, e.g. the `50`/`invert` midpoint Bulma chooses). The built-in theme colors (`primary`, `link`, `info`, `success`, `warning`, `danger`) each have a full palette.

### Registering a custom color (build-time, Sass)
Registering a new palette is a Sass-side concern: add the color to the `$colors` map (and/or `$shades`/`$grays`) BEFORE Bulma derives variables, via `@use ... with (...)` or a custom derived-variables file. After build, the 21 `--bulma-{name}-*` CSS variables exist and can be themed at runtime like any other.

```scss
@use "bulma/sass" with (
  $custom-colors: (
    "brand": (hsl(280, 90%, 60%)), // registers "brand" → 21 shades generated
  ),
);
```
(Exact map key/shape follows Bulma's `$colors` convention; confirm against bulma.io/documentation/customize/list-of-sass-variables/ for your point release.)

### Palette vs color helper vs theme
| Thing | Lives in | What it is |
|---|---|---|
| Palette CONCEPT + `--bulma-{name}-00..100` vars | `bulma-customize` (here) | The generated shade tokens + registration. |
| `palette-*` / `has-text-palette-*` CLASSES | `bulma-helpers` | Apply a shade to an element. |
| The 8 theme colors (primary, link, info, success, warning, danger, light, dark) (concept) | `bulma-fundamentals` | Names + hex of the default colors. |
| A theme | `bulma-customize` (here) | A named batch of `--bulma-*` overrides. |

## Putting it together: light/dark + custom theme + palette
```html
<html class="theme-dark">            <!-- built-in dark -->
  <div class="brand-cyber">          <!-- custom theme, overrides dark inside -->
    <button class="button is-primary palette-primary-70">
      Primary button at the 70% palette shade
    </button>
  </div>
</html>
```
(`palette-primary-70` is a `bulma-helpers` class; `--bulma-primary-70` is the variable this skill's palette concept produced.)

## References
- https://bulma.io/documentation/features/dark-mode/
- https://bulma.io/documentation/features/themes/
- https://bulma.io/documentation/features/color-palettes/
- Palette application classes: see `bulma-helpers` skill (`references/color-typography.md`)
