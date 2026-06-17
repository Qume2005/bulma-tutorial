# Baseline-Failure Artifact: bulma-fundamentals

## Context
Per `development-team:writing-skills` (TDD-for-docs), this is the concrete wrong/outdated Bulma pattern that `bulma-fundamentals` must prevent. Written BEFORE the skill so the skill is the minimal doc that closes it.

## The failure (what a user does today, wrong/stale)

A user picks up a Bulma tutorial from the 0.9 era and copies the setup snippet:

```html
<!-- WRONG (stale 0.9 advice) -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css">

<style>
  /* User believes theming = override the Sass value directly in the browser.
     They write inline rules because they don't know Bulma v1 exposes CSS variables. */
  .button.my-brand {
    background-color: #00d1b2 !important; /* hardcoded turquoise, unthemed at runtime */
  }
</style>
```

They also misunderstand the modifier syntax and write a button as:

```html
<!-- WRONG syntax: bare color name, no is- prefix; renders unstyled -->
<button class="button primary large">Sign up</button>
```

…expecting a teal, large button. It renders as a plain default button because Bulma modifiers require the `is-` prefix (`is-primary`, `is-large`), and colors are not bare class names.

And when asked "is Bulma responsive?" they don't know Bulma ships **5 named screen sizes** (mobile, tablet, desktop, widescreen, fullhd) with fixed pixel breakpoints, so they hand-roll media queries instead of using Bulma's built-in responsive behavior.

## Why this fails (the knowledge gap)

1. **CDN URL / version drift** — the user pins `bulma@0.9.4` and misses v1 (CSS variables, `.block`, Grid, built-in dark mode). They don't know v1 exists or that the single-file install is still valid at v1.0.4.
2. **Wrong theming mental model** — in v1, the runtime theming mechanism is overriding `--bulma-*` CSS variables (e.g. `--bulma-primary`), NOT hardcoding hex into custom classes. Hardcoding defeats the whole point of Bulma v1's variable system and breaks dark mode / palette generation.
3. **Modifier syntax not understood** — Bulma's grammar is `.element` + `is-state`/`is-modifier`/`has-modifier` + color names that are also `is-*` (`is-primary`, `is-info`, ...). A bare `primary`/`large` is not a Bulma class. Without knowing the `is-`/`has-` ecosystem, the user cannot read or write Bulma at all.
4. **Responsive system invisible** — not knowing the 5 breakpoints exist, the user re-invents responsive CSS instead of leaning on Bulma's `is-*` responsive variants and container behavior.

## The correction this skill must deliver (the GREEN state)

`bulma-fundamentals` teaches, minimally and correctly:

- **Install** — the three current ways (CDN single CSS file / npm / Sass source), and that v1.0.4 is the target. Pin to the v1 CDN URL.
- **Syntax** — `.element` + `is-*`/`has-*` modifiers; a button is `<button class="button is-primary is-large">`, never bare color words.
- **The 8 theme colors** (concept level only): primary, link, info, success, warning, danger, plus light/dark (and the base palette they derive from: turquoise/cyan/green/yellow/red/blue). Used as `is-<color>`. (`white`/`black` are base values and `grey` lives in the `$shades` map — none of these three is a theme color.)
- **The 5 breakpoints** with exact pixel values (mobile ≤768, tablet 769–1023, desktop 1024–1215, widescreen 1216–1407, fullhd ≥1408) and the `is-*-only` variants concept — full table pushed to `references/responsiveness.md`.
- **Brief CSS-variables intro** — one paragraph: "Bulma v1 themes via `--bulma-*` CSS variables; Sass compiles down to them. To actually override them or set up dark mode, see `bulma-customize`." NOT the full override recipe (that's customize's job — DoD box 13).
- **Ownership hand-off** — deep theming / Sass / dark mode / migration → `bulma-customize`. Layout (columns/grid/container/hero) → `bulma-layout`. Elements/components/forms/helpers have their own skills.

The skill closes all four failure points above. It does NOT cover the full modifier catalog (→ `references/modifiers-and-colors.md`), the full breakpoint table (→ `references/responsiveness.md`), or any deep theming (→ `bulma-customize`).

## References (sources verifying the corrections)
- https://bulma.io/documentation/start/installation/ — CDN single CSS file, npm install, Sass source
- https://bulma.io/documentation/start/responsiveness/ — the 5 breakpoints + exact pixel values
- https://bulma.io/documentation/features/css-variables/ — `--bulma-*` are primary theming in v1, Sass compiles down to CSS variables
- https://bulma.io/documentation/start/overview/ — syntax, modifiers, color overview
- https://bulma.io/documentation/helpers/ — the `is-*`/`has-*` modifier ecosystem
