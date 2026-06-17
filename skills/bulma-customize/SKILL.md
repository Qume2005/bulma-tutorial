---
name: bulma-customize
description: Customizing Bulma. Use when the user asks how to customize Bulma colors, fonts, or spacing, how to override Bulma Sass variables, how to use modular Sass (@use "bulma/sass" with (...), @forward), that Dart Sass is required (LibSass/node-sass unsupported in Bulma 1.0), how to set up Bulma dark mode or switch themes, how to override Bulma CSS variables (--bulma-*), how to define custom color palettes, or how to migrate from Bulma 0.9 to 1.0. Advanced theming and build configuration.
---

# Customizing Bulma (v1.0.4)

## When to use this skill
- "How do I change Bulma's primary color / brand color?" → runtime: override `--bulma-*`; build-time: `@use ... with (...)`.
- "How do I override a Bulma Sass variable like `$primary`, `$family-sans-serif`, `$column-gap`?"
- "Does Bulma need Dart Sass? Why won't node-sass / LibSass work?" → yes, Dart Sass is required.
- "How do I use `@use "bulma/sass" with (...)` or `@forward` instead of `@import`?"
- "How do I turn on Bulma dark mode, or switch light/dark themes at runtime?"
- "How do I define/register a custom color palette (21 shades)?" — this skill owns the CONCEPT and the variables.
- "How do I migrate a Bulma 0.9 project to 1.0?" (`@import`→`@use`, `$var`→`--bulma-var`, LibSass dropped).
- **Out of scope:** the 8 theme colors (primary, link, info, success, warning, danger, light, dark) at a concept level → `bulma-fundamentals`. Applying an EXISTING palette via `palette-*` helper classes → `bulma-helpers` (this skill owns the palette CONCEPT/variables, not the helper classes). The CSS Grid system → `bulma-layout`. The `.block` element → `bulma-elements`.

## Key concepts (the v1 mental model — read this first)
- **`--bulma-*` CSS variables are the PRIMARY theming mechanism in Bulma 1.0.** Every Bulma component reads its colors, spacing, radius, and shadows from CSS custom properties like `--bulma-primary`, `--bulma-scheme-main`, `--bulma-radius`. This is the single most important v1 fact.
- **Sass variables compile DOWN to `--bulma-*` CSS variables at build time.** A Sass `$primary` becomes `--bulma-primary` in the output CSS. To theme at RUNTIME (toggle dark mode, live color picker, user preference) you override the CSS variable — you do NOT need to rebuild Sass.
- **Dark mode is built-in (no plugin).** Bulma ships light and dark default themes, reacts to `prefers-color-scheme`, and can be toggled with `.theme-dark` / `.theme-light` classes.
- **Dart Sass is REQUIRED.** LibSass and node-sass are unsupported/deprecated. The entry point is `@use "bulma/sass" with (...)` and `@forward`, NOT `@import`.
- **A theme = a collection of `--bulma-*` variables.** Bulma ships 2 default themes (light, dark); you define more by overriding the variables under a selector.
- **A color palette = the 21 auto-generated lightness shades (0%–100% in 5% steps) of a registered color**, exposed as `--bulma-{color}-{00..100}` CSS variables. This skill owns the concept + registration; `bulma-helpers` owns the `palette-*` classes that APPLY a palette.
- **0.9 → 1.0 migration is non-trivial:** `@import`→`@use`, Sass `$variable`→CSS `--bulma-variable` as the runtime value, LibSass dropped, CSS Grid + `.block` added.

## Common patterns (copy-paste)

### 1. Override a theme color at runtime (no rebuild) — the v1 default
```css
/* Anywhere AFTER bulma.css is loaded */
:root {
  --bulma-primary-h: 171deg;   /* HSL components drive the derived shades */
  --bulma-primary-s: 100%;
  --bulma-primary-l: 41%;
}
```
Override on a scoped element to theme a subtree: `.my-card { --bulma-primary: hsl(...); }`.

### 2. Build-time theming with Dart Sass (`@use ... with (...)`)
```scss
// styles.scss — compile with: sass styles.scss styles.css  (Dart Sass)
@use "bulma/sass" with (
  $family-sans-serif: ("Inter", "Arial", sans-serif),
  $primary: hsl(171, 100%, 41%),
  $column-gap: 1.25rem,
);
```
Note: these Sass overrides still compile DOWN to `--bulma-*` CSS variables in the output.

### 3. Import only the Sass you need (modular)
```scss
@use "bulma/sass/base/minireset";
@use "bulma/sass/elements/button";
@use "bulma/sass/components/navbar";
/* skip columns, form, etc. — smaller CSS */
```

### 4. Forward a customized Bulma for a library/team
```scss
// my-brand.scss
@forward "bulma/sass" with (
  $primary: #ff0000,
  $link: #0066ff,
);
```

### 5. Toggle dark mode at runtime (built-in)
```html
<html class="theme-dark">  <!-- force dark -->
<html class="theme-light"> <!-- force light -->
<html>                     <!-- auto: follows prefers-color-scheme -->
```
Toggle the class from JS; no rebuild, no plugin.

### 6. Register a custom color palette (concept) so its 21 shades exist
Palette registration is done in Sass (build-time); see `references/dark-mode-themes.md` for the variable map and `references/css-variables.md` for the resulting `--bulma-{name}-00..100` CSS variables. Applying a shade to an element uses `palette-*` helper classes owned by `bulma-helpers`.

## v1 gotchas
- **`--bulma-*` is the runtime theming surface.** Editing a Sass `$variable` and rebuilding is the BUILD-TIME path; if a user just wants to change a color live, override the CSS variable. (Source: bulma.io/documentation/customize/with-css-variables/ and /features/css-variables/.)
- **Dart Sass required; `@import` is legacy.** Bulma 1.0's Sass uses `@use`/`@forward` with directory index files that LibSass/node-sass cannot resolve. Use the `sass` npm package. (Source: bulma.io/documentation/customize/with-sass/.)
- **Dark mode is built-in** via `prefers-color-scheme` + `.theme-dark`/`.theme-light`. No JS framework, no plugin. (Source: bulma.io/documentation/features/dark-mode/.)
- **HSL-based variables:** Bulma stores colors as HSL components (`--bulma-primary-h/s/l`); derived shades reference them. Override the `-h/-s/-l` trio (or the computed `--bulma-primary`) rather than expecting a single hex override to cascade. (Source: bulma.io/documentation/features/css-variables/.)
- **Themes = a collection of `--bulma-*` variables;** 2 default themes ship. (Source: bulma.io/documentation/features/themes/.)
- **Color palettes auto-generate 21 lightness shades** per registered color; the CONCEPT and registration live here, the `palette-*` application classes live in `bulma-helpers`. (Source: bulma.io/documentation/features/color-palettes/.)
- **0.9 → 1.0 migration is non-trivial:** `@import`→`@use`, Sass `$var`→`--bulma-var`, LibSass dropped, CSS Grid + `.block` added. (Source: bulma.io/documentation/start/migrating-to-v1/.)
- **Do NOT confuse with siblings:** CSS Grid (→ `bulma-layout`), `.block` element (→ `bulma-elements`), palette-* helper classes (→ `bulma-helpers`), the 8 theme colors (primary, link, info, success, warning, danger, light, dark) concept (→ `bulma-fundamentals`).

## References (load on demand)
Load these bundled files when you need full variable catalogs, Sass syntax, theme tables, or the migration checklist. Every file below is part of this skill:
- `references/css-variables.md` — The full `--bulma-*` surface (colors, scheme, spacing, radius, shadows, typography), how Sass vars compile down to them, HSL-component pattern, scoped overrides, and why CSS vars are the primary v1 theming mechanism.
- `references/sass.md` — Dart Sass requirement (LibSass/node-sass unsupported), `@use "bulma/sass" with (...)`, `@forward`, modular imports, key Sass variable overrides (`$primary`, `$family-sans-serif`, `$column-gap`, derived/global vs component variables), and common build setups (sass CLI / Vite / webpack).
- `references/dark-mode-themes.md` — Built-in dark mode, `prefers-color-scheme`, `.theme-dark`/`.theme-light`, runtime theme switching, defining custom themes by overriding `--bulma-*`, and how `features/themes` + `features/color-palettes` relate.
- `references/migration.md` — The 0.9 → 1.0 migration: `@import`→`@use`, Sass `$var`→`--bulma-var`, LibSass dropped, deprecated/removed/renamed variables, new CSS Grid + `.block`, and a step-by-step upgrade checklist.
