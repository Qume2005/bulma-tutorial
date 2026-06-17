# Migrating Bulma 0.9 → 1.0 — Reference

> Scope: the non-trivial 0.9 → 1.0 upgrade. Sourced from https://bulma.io/documentation/start/migrating-to-v1/.

## Why this is non-trivial

Bulma 1.0 is a major theming-system rewrite. The four breaking axes:

1. **Sass module system:** `@import` → `@use`/`@forward`.
2. **CSS variables as the runtime value:** Sass `$variable` now compiles DOWN to `--bulma-variable`; runtime theming moves to CSS vars.
3. **Dart Sass required:** LibSass / node-sass dropped (can't resolve Bulma's `_index.scss` module structure).
4. **New primitives:** CSS Grid (`grid`/`cell`/`smart-grid`/`fixed-grid`, owned by `bulma-layout`) and the `.block` spacing element (owned by `bulma-elements`).

Plus minor renames, deprecations, and a couple of removed classes.

## Pre-flight
- Read https://bulma.io/documentation/start/migrating-to-v1/ end to end.
- Pin a clean git state; do this on a branch.
- Identify your Sass entry file(s) and build tool.

## Step-by-step checklist

### 1. Sass toolchain
- [ ] Replace `node-sass`/`ruby-sass`/LibSass with Dart Sass: `npm install --save-dev sass`.
- [ ] Confirm `sass --version` reports Dart Sass.
- [ ] Update `sass-loader` (webpack) / your bundler config to the modern API and the `sass` implementation.

### 2. Syntax: `@import` → `@use`/`@forward`
- [ ] Replace `@import "bulma/bulma";` with:
      ```scss
      @use "bulma/sass" with ( /* overrides here */ );
      ```
- [ ] Move EVERY variable override INTO the `with (...)` clause. Assignments AFTER `@use` no longer take effect (this is the #1 migration bug).
      ```scss
      // 0.9 (BROKEN in 1.0)
      @import "bulma/bulma";
      $primary: red;        // too late

      // 1.0 (correct)
      @use "bulma/sass" with ($primary: red);
      ```
- [ ] For partial imports, replace each `@import "bulma/sass/elements/button"` with `@use "bulma/sass/elements/button"`. Note member namespacing changed — you no longer get globals leaked into your file.
- [ ] For libraries re-exporting Bulma, use `@forward "bulma/sass" with (...)`.

### 3. Theming model: Sass `$var` → CSS `--bulma-var`
- [ ] Audit custom CSS that hardcoded a Bulma color value (e.g. `color: #00d1b2;`). Prefer `color: var(--bulma-primary);` so it tracks theming.
- [ ] If you had runtime theming hacks (extra compiled variants, JS-injected `<style>`), replace with `--bulma-*` overrides under a selector. See `css-variables.md`.
- [ ] Decide your strategy per project: Sass build (custom default) OR runtime CSS vars (ship prebuilt). You rarely need both for one token.
- [ ] Remember the HSL-component gotcha: overriding `--bulma-primary` alone may not move the derived `-light`/`-dark`/`-invert` and the 21-shade palette; override the `-h`/`-s`/`-l` trio for full cascade.

### 4. Dark mode (new — usually a win)
- [ ] You now get built-in dark mode for free. Decide your default: auto (`prefers-color-scheme`), forced (`<html class="theme-dark">`), or a JS toggle.
- [ ] See `dark-mode-themes.md` for the toggle snippet and custom-theme recipe.

### 5. Variables renamed / removed / changed
- [ ] Check bulma.io/documentation/customize/list-of-sass-variables/ against your overrides. Common 0.9→1.0 changes:
      - Color initial values are HSL, not hex (hex still works; Bulma converts).
      - Some `derived-variables` keys moved; `$colors` map shape follows the new palette system.
      - Generic spacing tokens map to `--bulma-*` (e.g. `$column-gap` → `--bulma-column-gap`).
- [ ] Remove overrides for variables that no longer exist; the Sass compiler will warn on unknown `with (...)` keys.

### 6. New primitives (reference — owned by sibling skills)
- [ ] CSS Grid (`grid`, `cell`, `smart-grid`, `fixed-grid`, `is-col-min-*`) — see `bulma-layout`. Consider migrating dense 12-col dashboards from `columns` to `grid`.
- [ ] `.block` element — see `bulma-elements`. Replace ad-hoc `margin-top`/`<br>` spacers between stacked elements with `<div class="block">`.

### 7. Build & verify
- [ ] Build once; fix deprecation warnings (each `@import`, each LibSass-ism).
- [ ] Visually diff key pages (navbar, hero, buttons, forms, tables, modal).
- [ ] Confirm dark-mode toggle works if you enabled it.
- [ ] Run your test suite / screenshot tests.

## The most common single failure

> A dev follows a 0.9 tutorial, writes `@import "bulma/bulma"; $primary: #ff0000;`, rebuilds, and the color does not change. In 1.0 the fix is one of:
> - Build-time: `@use "bulma/sass" with ($primary: #ff0000);` (Dart Sass).
> - Runtime: `:root { --bulma-primary: hsl(0, 100%, 50%); }` (no rebuild).
> This is the baseline failure the whole `bulma-customize` skill exists to prevent.

## Quick mapping table

| 0.9 | 1.0 |
|---|---|
| `@import "bulma/bulma"` | `@use "bulma/sass" with (...)` |
| `$primary: red;` after import | inside `with (...)`, or override `--bulma-primary` |
| node-sass / LibSass | Dart Sass (`sass` package) |
| No dark mode | built-in: `prefers-color-scheme` + `.theme-dark`/`.theme-light` |
| No CSS Grid | `grid`/`cell`/`smart-grid`/`fixed-grid` (→ `bulma-layout`) |
| `margin-top` hacks | `.block` element (→ `bulma-elements`) |
| Hardcoded color values in CSS | `var(--bulma-*)` |
| No color palettes | 21-shade `--bulma-{color}-00..100` auto-generated |

## References
- https://bulma.io/documentation/start/migrating-to-v1/
- https://bulma.io/documentation/start/migrating-to-v0-9/ (intermediate 0.8→0.9 context)
- Internal: `css-variables.md`, `sass.md`, `dark-mode-themes.md`
