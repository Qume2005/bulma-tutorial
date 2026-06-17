# Baseline Failure: bulma-helpers

## Context

This artifact captures the concrete, real-world failure that the `bulma-helpers` skill must close. Per `development-team:writing-skills` (TDD-for-docs), the failure is shown here BEFORE the skill's correction. The skill is then the minimal doc that makes this failure impossible.

## The Failure (what users do without this skill)

A developer building a Bulma page routinely reaches for inline `style=""` attributes or hand-rolled CSS for things Bulma already ships as one-class utilities. Concretely, the anti-pattern looks like this:

```html
<!-- Anti-pattern: inline styles for color, spacing, alignment, and hiding -->
<p style="color: #3273dc; font-weight: 700; text-align: center;">Saved successfully</p>

<div style="margin-top: 1.5rem; padding: 0 1.25rem;">
  <span style="float: left;">Avatar</span>
  <span style="display: none;" data-mobile-only>Mobile badge</span>
</div>

<!-- Loading state: a hand-rolled grey pulse div instead of a skeleton -->
<div class="fake-loader" style="background: #dbdbdb; height: 1.25rem; border-radius: 4px;"></div>
```

This is wrong in Bulma v1.0.4 for several compounding reasons:

1. **Inline styles defeat responsive design.** A `style="color:#3273dc"` hard-codes the hue; the Bulma helper `has-text-link` resolves to `--bulma-link` (a CSS variable), so it tracks theme/dark-mode overrides automatically. Inline styles silently break dark mode.
2. **Magic numbers replace a documented scale.** `margin-top: 1.5rem` is a guess. Bulma ships a numbered spacing scale (`mt-4`, `pb-5`, `mx-6`) that is consistent across the whole page and has responsive variants (`mt-4-tablet`).
3. **Responsive behavior is impossible inline.** `display: none` cannot be scoped to "mobile only" without media queries. Bulma's `is-hidden-mobile` / `is-hidden-tablet-only` do exactly that, declaratively.
4. **The loading placeholder is a first-class v1 feature.** A grey div is not a skeleton; `is-skeleton` (applied to any element/component) or Bulma's pre-built skeleton utility elements are the v1-correct loaders — animated, themed, and consistent.
5. **Alignment, weight, size, case, and font-family** all have one-class helpers (`has-text-centered`, `has-text-weight-bold`, `is-size-4`, `is-uppercase`, `is-family-code`) that compose with the responsive breakpoint system.

## Why a per-element skill (or no skill) fails to close this

- `bulma-elements` documents elements like `notification` and `tag` but does NOT catalog the cross-cutting `has-text-*` / `m*` / `is-hidden-*` utilities that apply to ANY element.
- `bulma-customize` owns the palette *concept* and `--bulma-*` *variable* definitions, but NOT the `palette-*` helper *classes* nor the day-to-day utility classes a developer drops on a `<p>`.
- Without a dedicated helpers skill, an agent either (a) hallucinates inline-style workarounds, (b) reinvents a spacing scale, or (c) misses that `is-skeleton` exists at all (it's a v1 feature, easy to assume Bulma "has no skeletons").

## What closes the failure

A skill that, on the trigger phrases `has-text-*`, `has-background-*`, `spacing`, `mt-4`, `is-hidden-mobile`, `is-flex`, `skeleton`, `utility classes`, surfaces:

- The color helper catalog (`has-text-*` / `has-background-*` over the **8 theme colors** — primary, link, info, success, warning, danger, light, dark — plus light/dark variants) tied to `--bulma-*` so users stop hard-coding hex.
- The typography helper catalog (`is-size-1..7`, `has-text-weight-*`, `has-text-centered`/`-left`/`-right`/`-justified`, `is-uppercase`/`-lowercase`/`-capitalized`/`-italic`, `is-family-*`).
- The spacing scale (`m*`/`p*`, `mx/my/px/py/mr/ml/pr/pl`, values `0..6` and `auto`, responsive variants) so magic numbers are never needed.
- The flexbox helpers (`is-flex`, direction/wrap/justify/gap helpers).
- The visibility/display helpers (`is-hidden`, `is-hidden-mobile`/`-tablet-only`/…, `is-block`/`is-inline`/`is-flex` and responsive variants) so responsive hide/show is declarative.
- The `palette-*` helper CLASSES (applying an existing palette) — explicitly contrasted with `bulma-customize`, which owns the palette *concept* and variable *definitions*.
- Skeletons (`is-skeleton` / `has-skeleton` on any element/component, plus the pre-built skeleton utility elements) — a v1 feature this skill owns exclusively.

With this skill, the anti-pattern snippet above becomes:

```html
<p class="has-text-link has-text-weight-bold has-text-centered">Saved successfully</p>

<div class="block px-5">
  <span class="is-pulled-left">Avatar</span>
  <span class="is-hidden-tablet">Mobile badge</span>
</div>

<div class="is-skeleton" style="height: 1.25rem;"></div>
```

No inline colors, no magic spacing, no media queries, a real skeleton.

## Acceptance criterion

The `bulma-helpers` skill is the MINIMAL doc that closes this failure: lean `SKILL.md` (~1.5–2k words) + two `references/*.md` files carrying the full catalogs (color-typography, spacing-flex-visibility). It must state "8 theme colors" (NOT 9, NOT including `grey`/`white`/`black` as theme colors), and it must hand palette-concept/variable-definition and deep theming off to `bulma-customize`.
