# Baseline-Failure Artifact — `bulma-customize`

## Context
Per `development-team:writing-skills` (TDD-for-docs), this file captures a REAL, specific, outdated/wrong Bulma pattern that the `bulma-customize` skill must prevent. It is written BEFORE the skill so the skill is provably the minimal doc that closes the failure.

## The failure (stale 0.9-era theming advice applied to Bulma 1.0.4)

A developer follows a popular 2021 Bulma 0.9 tutorial that says:

> "To change Bulma's primary color, override `$primary` in your Sass entry file and recompile."

They write `styles.scss`:

```scss
// WRONG for Bulma 1.0 — this is the 0.9 mental model
@import "bulma/bulma";          // (a) @import is legacy Sass; Bulma 1.0 uses @use
$primary: #ff0000;              // (b) declared AFTER the import, so it has no effect
                                 // (c) even if it compiled, the runtime CSS still uses
                                 //     --bulma-primary, NOT the Sass $primary value,
                                 //     because Sass vars compile DOWN to CSS vars in v1
```

Then a teammate wants to switch to dark mode at runtime by toggling a class on `<html>`. The developer has no path: their mental model is "edit Sass, rebuild", which cannot react to a runtime class toggle or to `prefers-color-scheme`.

### Concretely broken outcomes
1. **No color change appears** — `$primary` is assigned after `@import`, and Bulma 1.0 doesn't even read it at runtime (CSS var `--bulma-primary` drives the components).
2. **`@import` triggers Sass deprecation warnings** under Dart Sass; under node-sass/LibSass it may silently produce wrong output. Bulma 1.0 officially requires Dart Sass and `@use ... with (...)`.
3. **Dark mode is unreachable** — the developer doesn't know Bulma ships built-in dark mode (`.theme-dark` / `.theme-light` + `prefers-color-scheme`) because the 0.9 tutorial predates it.
4. **Migration is blocked** — the project is on Bulma 0.9 and the dev assumes "just bump the version"; the `@import`→`@use`, Sass-var→CSS-var, and LibSass-dropped changes break the build.

## What the correct v1 flow looks like (the skill must teach this)

**Runtime theming (primary mechanism in v1) — no rebuild:**
```css
:root {
  --bulma-primary: hsl(171deg, 100%, 41%); /* override the CSS variable directly */
}
/* Dark mode is built in: */
html.theme-dark { /* ... */ }   /* or rely on prefers-color-scheme */
```

**Build-time theming (Sass) — requires Dart Sass, uses `@use ... with (...)`:**
```scss
@use "bulma/sass" with (
  $primary: #ff0000,
  $family-sans-serif: "Inter", sans-serif,
);
```

**Migration:** `@import` → `@use`, Sass `$variable` compiles down to `--bulma-variable`, LibSass/node-sass unsupported, new CSS Grid + `.block` added.

## Why this is the RIGHT baseline failure for this skill
- It is the single most common, highest-impact confusion in Bulma 1.0 theming (the contract calls it "the most important one in the collection").
- It spans ALL four reference files this skill owns: `css-variables.md` (the override mechanism), `sass.md` (Dart Sass + `@use ... with`), `dark-mode-themes.md` (built-in dark mode), and `migration.md` (0.9→1.0).
- It is specific: named classes (`--bulma-primary`, `$primary`), named rules (`@import` vs `@use`), a quoted stale claim, and a concrete broken `styles.scss`.

## Minimal skill that closes it
`SKILL.md` states the v1 mental model up front ("Sass compiles DOWN to `--bulma-*`; override the CSS var at runtime"), names Dart Sass + `@use ... with (...)` for build-time, points to dark mode/themes/palettes as built-in, and hands the deep tables/syntax/migration checklist to the four `references/*.md` files. No gold-plating beyond customize's scope.
