# Bulma CSS Variables (`--bulma-*`) — Reference

> Scope: the PRIMARY v1 theming mechanism. Sourced from https://bulma.io/documentation/customize/with-css-variables/ and https://bulma.io/documentation/features/css-variables/.

## The core idea (read first)

In Bulma 1.0, **every component reads its design tokens from CSS custom properties prefixed `--bulma-`**. Sass variables (`$primary`, `$radius`, etc.) compile DOWN to these CSS variables at build time. The compiled output is effectively:

```css
:root {
  --bulma-primary: hsl(171deg, 100%, 41%);
  --bulma-radius: 0.375rem;
  /* ...hundreds more... */
}
.button.is-primary { background-color: var(--bulma-primary); }
```

Consequence: **to change Bulma's appearance at runtime you override the CSS variable — no Sass rebuild needed.** This is what makes live theming, dark mode, and `prefers-color-scheme` work.

## Why this matters vs the 0.9 model

| Bulma 0.9 | Bulma 1.0 |
|---|---|
| Override Sass `$primary`, recompile → new value baked into `.button` rule. | Override CSS `--bulma-primary` at `:root` or scoped → `.button` picks it up live. |
| Theming requires a build step. | Theming can be pure CSS (runtime). |
| `@import "bulma"`; | `@use "bulma/sass" with (...)`; |

## The HSL-component pattern (gotcha)

Bulma stores most colors as **HSL components** so the 21-shade palettes and `*-light`/`*-dark` variants can be derived. A single hex override often won't cascade. Override the trio (or the computed value):

```css
:root {
  --bulma-primary-h: 340deg;
  --bulma-primary-s: 90%;
  --bulma-primary-l: 45%;
  /* computed convenience var */
  --bulma-primary: hsl(var(--bulma-primary-h), var(--bulma-primary-s), var(--bulma-primary-l));
}
```

Derived variables reference these: `--bulma-primary-light`, `--bulma-primary-dark`, `--bulma-primary-invert`, etc.

## Variable surface (categories)

Bulma exposes hundreds of `--bulma-*` variables. The categories below are the ones you'll most often override. (For the exhaustive list, see the table at bulma.io/documentation/customize/with-css-variables/.)

### Colors / scheme
- `--bulma-primary`, `--bulma-link`, `--bulma-info`, `--bulma-success`, `--bulma-warning`, `--bulma-danger`
- Each has `-h`/`-s`/`-l` HSL components and derived `-light`/`-dark`/`-invert`/`-on-*` variants.
- Scheme (page background/text): `--bulma-scheme-main`, `--bulma-scheme-main-ter`, `--bulma-scheme-invert`, `--bulma-text`, `--bulma-text-strong`, `--bulma-background`.

### Spacing
- `--bulma-column-gap` (default `0.75rem`), `--bulma-block-spacing` (the `.block` element rhythm), `--bulma-section-padding` / `-medium` / `-large`.

### Radius
- `--bulma-radius-small` `0.25rem`, `--bulma-radius` `0.375rem`, `--bulma-radius-large` `0.5rem`, `--bulma-radius-rounded` `9999px`.

### Shadows
- `--bulma-shadow` (and `-h`/`-v`/`-blur`/`-spread`/`-color` components).

### Typography
- `--bulma-family-primary`, `--bulma-family-secondary`, `--bulma-family-code`, `--bulma-family-sans-serif`, `--bulma-family-monospace`.
- Sizes: `--bulma-size-1` ... `--bulma-size-7`, weights `--bulma-weight-light/normal/medium/semibold/bold`.

### Generic / derived
- `--bulma-border` (border color), `--bulma-control-radius`, `--bulma-button-*`, component-specific tokens.

## How to override

### Global (whole site) — `:root`
```css
:root {
  --bulma-primary: hsl(340, 90%, 45%);
  --bulma-radius: 0.75rem;       /* rounder everything */
}
```

### Scoped (one subtree) — any selector
```css
.checkout-card {
  --bulma-primary: hsl(150, 70%, 40%); /* green only inside this card */
}
```
Because `var()` cascades, all Bulma components inside `.checkout-card` pick up the new primary.

### Theme blocks (see also dark-mode-themes.md)
```css
html.theme-dark { /* override a batch of --bulma-* for dark mode */ }
.brand-cool      { /* a named custom theme */ }
```

## Relationship to Sass variables

- Sass `$primary` → compiled to `--bulma-primary`.
- Overriding the Sass var changes the BUILD-TIME default; overriding the CSS var changes the RUNTIME value.
- If you only ship the prebuilt `bulma.css`, Sass overrides are unavailable to you — use CSS var overrides. See `sass.md` for the build-time path.
- Gotcha (community-reported): some overrides require the themes folder to be present in your build; if a CSS var override "does nothing", confirm you're loading Bulma's theme CSS and that your override comes AFTER it in the cascade.

## Verifying your override
1. Open DevTools → inspect a `.button.is-primary`.
2. Confirm `background-color: var(--bulma-primary)`.
3. Edit `--bulma-primary` on `:root` in the Styles panel — the button updates instantly. This is the v1 theming loop.

## References
- https://bulma.io/documentation/customize/with-css-variables/
- https://bulma.io/documentation/features/css-variables/
- MDN: https://developer.mozilla.org/en-US/docs/Web/CSS/--*
