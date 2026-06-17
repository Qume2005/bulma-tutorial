---
name: bulma-fundamentals
description: Bulma setup and fundamentals. Use when the user asks how to install Bulma (npm/CDN/sass), how Bulma class syntax works (.element.is-state, .has-modifier), what Bulma modifiers are, the Bulma color system and the 8 theme colors (primary, link, info, success, warning, danger, light, dark), or Bulma responsive breakpoints (mobile, tablet, desktop, widescreen, fullhd). Also use for the is-*/has-* modifier ecosystem and a brief intro to Bulma CSS variables. The foundation every Bulma page needs; hands deep theming/dark-mode/themes off to bulma-customize.
---

# Bulma Fundamentals: Setup, Syntax, Modifiers, Colors, Breakpoints

## When to use this skill
- "How do I install Bulma?" / "include Bulma via CDN / npm / Sass"
- "How does Bulma class syntax work?" / "what is a Bulma modifier?" / "is-* / has-* classes"
- "What are Bulma's colors?" / "the 8 theme colors" / "is-primary, is-info, is-danger…"
- "Is Bulma responsive? What are the breakpoints?" / "mobile vs tablet vs desktop vs widescreen vs fullhd"
- "What are Bulma CSS variables?" / "the --bulma-* thing"
- **OUT of scope — route to siblings:** deep theming, overriding `--bulma-*`, Sass `@use ... with (...)`, Dart Sass, dark mode, themes, color palettes, 0.9→1.0 migration → `bulma-customize`. Layout (columns, grid, container, hero, section, footer) → `bulma-layout`. The full helper utility catalog (`is-size-*`, `has-text-*`, `has-text-weight-*`, spacing `m*`/`p*`, `is-hidden-*`, skeletons) → `bulma-helpers`. Individual elements/components/forms → their own skills.

## Key classes & concepts
- **Single CSS file** — Bulma's sole build output is one `bulma.css`. You can use it as-is (CDN/npm) or compile from the Sass source.
- **`.element`** — every Bulma component/element is a base class (e.g. `button`, `notification`, `columns`).
- **`is-*` / `has-*` modifiers** — modifiers are added to the base class to change state, size, color, style. Grammar: `<div class="element is-modifier">`. Modifiers are NOT bare words — `class="button primary"` does nothing; it must be `class="button is-primary"`.
- **8 theme colors** (used as `is-<color>` / `has-text-<color>`): `primary`, `link`, `info`, `success`, `warning`, `danger`, plus `light` and `dark`. (Internally these derive from a base palette: turquoise→primary, blue→link, cyan→info, green→success, yellow→warning, red→danger. `white`/`black` are base values and `grey` lives in the `$shades` map — neither is a theme color.)
- **5 breakpoints** — mobile, tablet, desktop, widescreen, fullhd. Mobile-first; each larger breakpoint has an `*-only` variant. Full table in `references/responsiveness.md`.
- **`--bulma-*` CSS variables** — in v1, Bulma styles everything via CSS custom properties (e.g. `--bulma-primary`, `--bulma-link`). Sass variables compile DOWN to these CSS variables. **Brief intro only here** — to actually override them or set up dark mode, see `bulma-customize`.
- **Modular Sass imports** — you can import only the parts of Bulma you need (`@use "bulma/sass/elements/button"`). Deep usage → `bulma-customize`.
- **No JavaScript framework ships with Bulma** — interactivity (navbar toggle, modal open, dropdown) is a few lines you add. (Owned by `bulma-components`.)

## Common patterns (copy-paste)

Install via CDN (v1 single CSS file):
```html
<link rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bulma@1.0.4/css/bulma.min.css">
```

Install via npm, then import the single CSS:
```bash
npm install bulma
```
```js
// entry.js
import "bulma/css/bulma.min.css";
```

Modifier grammar — base class + `is-*` (NEVER bare color words):
```html
<button class="button is-primary is-large">Sign up</button>
<!-- is-primary = teal color, is-large = size. NOT class="button primary large" -->
```

Color modifiers on any element that supports colors:
```html
<span class="tag is-warning">Pending</span>
<div class="notification is-danger">Failed</div>
<div class="message is-info">…</div>
```

A responsive container (centers content, has max-widths per breakpoint):
```html
<div class="container">
  <p class="is-size-4-desktop">Shrinks to size-4 from desktop up.</p>
</div>
```

CSS variable intro — you can read or override `--bulma-*` at `:root` or scoped (full recipe in `bulma-customize`):
```css
/* concept only — deep theming is bulma-customize's job */
:root {
  --bulma-primary: /* the value behind is-primary */;
}
```

## v1 gotchas
- **v1.0.4 is the current line.** Old tutorials pinning `bulma@0.9.x` miss: CSS variables, built-in dark mode, the CSS Grid system, and the `.block` spacer element. Prefer the v1 CDN/npm URL.
- **Theming is `--bulma-*` CSS variables, not hardcoded hex.** In v1, runtime theming means overriding `--bulma-primary` etc.; hardcoding `background: #00d1b2` into a custom class defeats dark mode and palette generation. (The override recipe lives in `bulma-customize`.)
- **`is-*`/`has-*` is the ONLY modifier grammar.** Bulma has no bare color/size classes — `primary`, `large`, `success` as standalone classes do nothing. Every modifier is prefixed.
- **Breakpoints are fixed** — mobile ≤768px, tablet 769–1023px, desktop 1024–1215px, widescreen 1216–1407px, fullhd ≥1408px. Bulma is mobile-first: a modifier without a breakpoint suffix applies from mobile up; a suffixed one (`*-desktop`, `*-widescreen`) applies from that breakpoint up.
- **Migrating from 0.9 → 1.0** is non-trivial (`@import`→`@use`, CSS variables, LibSass dropped). That migration is owned by `bulma-customize`; fundamentals only flags it exists.

## References (load on demand)
Load these bundled files when you need the full modifier/color catalog or the breakpoint table. Every file below is part of this skill:
- `references/modifiers-and-colors.md` — the full `is-*`/`has-*` modifier ecosystem (states, sizes, styles, colors) and the 8 theme colors with their base-palette derivation and light/dark variants (exact values are deferred to `bulma-customize`).
- `references/responsiveness.md` — the breakpoint table with exact pixel values, the `is-*-only` variants, `is-widescreen`/`is-fullhd` behavior, and container max-widths per breakpoint.
