# Bulma Sass — Reference (Dart Sass required)

> Scope: build-time customization via Sass. Sourced from https://bulma.io/documentation/customize/with-sass/ and https://bulma.io/documentation/customize/with-modular-sass/.

## Dart Sass is REQUIRED

Bulma 1.0's Sass source uses the **module system** (`@use` / `@forward`) plus directory `_index.scss` files. **LibSass and node-sass cannot resolve these** and are officially unsupported/deprecated.

Install Dart Sass (the `sass` package):

```bash
npm install --save-dev sass
# or globally:
npm install -g sass
```

Verify you're on Dart Sass:
```bash
sass --version
```

If you see "LibSass" / node-sass in your toolchain, migrate it first (see `migration.md`).

## The entry points

| Goal | Entry | Syntax |
|---|---|---|
| Full Bulma, customized | `@use "bulma/sass" with (...)` | module `with` clause |
| Only some parts | `@use "bulma/sass/<part>"` per part | modular |
| Re-export customized Bulma | `@forward "bulma/sass" with (...)` | for libraries/teams |

### Full Bulma with overrides
```scss
// styles.scss
@use "bulma/sass" with (
  $family-sans-serif: ("Inter", "Arial", sans-serif),
  $primary: hsl(171, 100%, 41%),
  $link: hsl(217, 71%, 53%),
  $column-gap: 1.25rem,
  $radius: 0.5rem,
  $widescreen-enabled: false,
  $fullhd-enabled: false,
);
```

### Modular (import only what you need)
```scss
@use "bulma/sass/base/minireset.sass";
@use "bulma/sass/base/generic.sass";
@use "bulma/sass/elements/button.sass";
@use "bulma/sass/elements/title.sass";
@use "bulma/sass/components/navbar.sass";
// columns, form, etc. omitted → smaller CSS
```
Note: when going modular you must also pull in `base/` (reset + generic) and the `derived-variables` you reference, or components may be missing tokens. The full `@use "bulma/sass"` is the safe default; go modular only when bundle size matters.

### Forward (for a shared brand stylesheet)
```scss
// my-brand.scss — consumers then @use "my-brand"
@forward "bulma/sass" with (
  $primary: #ff0000,
  $link: #0066ff,
);
```

## Key Sass variables to override

### Global / base
- `$family-sans-serif`, `$family-monospace`, `$family-primary`, `$family-secondary`, `$family-code`
- `$body-background-color`, `$body-size`, `$body-rendering`
- `$radius-small` `.25rem`, `$radius` `.375rem`, `$radius-large` `.5rem`, `$radius-rounded`
- `$shadow` (and components)

### Colors (initial values)
- `$scheme-main`, `$scheme-invert`, `$background`, `$text`, `$text-light`, `$text-strong`
- `$primary`, `$info`, `$success`, `$warning`, `$danger`, `$link`
- Each becomes a `--bulma-*` CSS variable AND generates the 21-shade palette (see color-palettes via `dark-mode-themes.md`).

### Layout
- `$column-gap` (default `0.75rem`)
- `$section-padding`, `$section-padding-medium`, `$section-padding-large`
- `$hero-body-padding`, `$footer-background-color`

### Feature flags (set with `false` to drop output)
- `$widescreen-enabled`, `$fullhd-enabled`
- `$navbar-fixed-z`, etc. (component z-indexes)

### Derived variables
Colors set with `!default` flow into derived maps (`$colors`, `$shades`, `$grays`). Changing `$primary` ripples through `primary-light`, `primary-dark`, `primary-invert`, and the auto-generated palette automatically — you do not set derived vars by hand.

## Build setups

### sass CLI (simplest)
```bash
sass styles.scss:css/styles.css --style=compressed --no-source-map
```
Watch mode:
```bash
sass --watch styles.scss:css/styles.css
```

### Vite
`sass` is auto-detected for `.scss`/`.sass` files. Install `sass` as a devDep; `@use "bulma/sass" with (...)` in your entry `.scss` just works.

```js
// vite.config.js — only needed for custom include paths
export default {
  css: { preprocessorOptions: { sass: { /* api: 'modern-compiler' recommended */ } } },
};
```

### webpack (sass-loader)
Use `sass-loader` with the modern API and the Dart `sass` implementation. Avoid `node-sass`.

```js
module.exports = {
  module: {
    rules: [{ test: /\.s[ac]ss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }],
  },
};
```

## Sass → CSS var mapping (why both exist)

Every Sass `$variable` you override becomes a `--bulma-variable` in the compiled CSS. So:
- **Build-time default:** set the Sass var.
- **Runtime override (no rebuild):** set the CSS var (see `css-variables.md`).
- You usually pick ONE strategy per project: pure-CSS theming (ship prebuilt `bulma.css`, override `--bulma-*`) OR Sass build (custom compile). Mixing is fine but redundant for a single value.

## Common pitfalls
- **`$primary` after `@use` has no effect** — the `with (...)` clause is the only place overrides take effect; assigning `$primary: ...` after the `@use` is too late.
- **Using `@import`** — legacy, deprecated by Dart Sass, and Bulma's module structure won't resolve. Use `@use`/`@forward`.
- **Forgetting `_index.scss` resolution** — Dart Sass resolves directory imports via `_index`; LibSass does not. This is why LibSass is unsupported.
- **Hex color vs HSL** — Bulma computes derived shades from HSL. Passing a hex `$primary` works (Bulma converts), but for precise palette control pass `hsl(...)`.
- **`@use` namespaces** — `@use "bulma/sass"` does not leak members into your file's scope the way `@import` did; use `with (...)` for config, not member access.

## References
- https://bulma.io/documentation/customize/with-sass/
- https://bulma.io/documentation/customize/with-modular-sass/
- https://bulma.io/documentation/customize/concepts/
- https://bulma.io/documentation/customize/list-of-sass-variables/
- Sass module system: https://sass-lang.com/documentation/at-rules/use/
