# Baseline Failure — bulma-elements skill

## Context

This is the TDD-for-docs baseline-failure artifact for the `bulma-elements` skill (per `development-team:writing-skills`). It captures a concrete, real wrong/outdated Bulma pattern that a user hits today WITHOUT this skill, shown BEFORE the skill's correction. The skill must be the minimal doc that closes this failure.

The chosen failure centers on the **`.block` element, new in Bulma v1.0**, because it is the single most distinct v1 fact this skill owns, it is easily confused with the unrelated `.box` element, and the wrong pattern (legacy `<br>` / `margin-top` hacks) is extremely common in pre-v1 code and tutorials.

## The failure (what a user does today)

A developer is building a stacked Bulma page — a title, a notification, a progress bar, and a button — and wants consistent vertical breathing room between each block. Following a 0.9-era tutorial (or instinct), they write:

```html
<h1 class="title is-1">Dashboard</h1>

<br>

<div class="notification is-primary">All systems nominal</div>

<br><br>

<progress class="progress is-info" max="100" value="42"></progress>

<div style="margin-top: 1.5rem;">
  <button class="button is-primary">Refresh</button>
</div>
```

### Why this is wrong / fragile in v1

1. **`<br>` gives an uncontrollable, font-relative gap** that varies with the surrounding line-height and collapses or balloons unpredictably next to block elements. It is a line-break primitive, not a rhythm tool.
2. **Inline `style="margin-top: ..."`** bypasses Bulma's spacing scale entirely, is non-responsive, and fights any global rhythm resets.
3. **The result is visually inconsistent** — some gaps are one line tall, others are ad-hoc pixel values, none of them track each other.
4. The user never reaches the v1-correct answer because the 0.9 docs they read predate `.block`, and a search for "bulma spacing" lands them on unrelated helper utilities (`m*`/`p*`, owned by `bulma-helpers`) or the `.box` element (a completely different bordered container).

### The two-name confusion this failure exposes

The user also frequently asks, "Should I use `.box` or `.block` to space things?" — because both are three-letter words starting with `b`. Without a dedicated elements skill, an agent has nothing to disambiguate them:

- `.box` = a white, **bordered container** with padding, rounded corners, and a box shadow (a card-like surface). It does NOT add vertical rhythm between siblings.
- `.block` = a **vertical-rhythm spacer** (bottom margin) with no visible surface of its own. It does NOT draw a box.

Conflating them produces either a page full of unwanted shadows (using `.box` to space) or missing surfaces (using `.block` where a panel was wanted).

## The v1-correct answer (what the skill must teach)

Bulma 1.0 introduced the `.block` element as the standard vertical-rhythm spacer between adjacent elements:

```html
<h1 class="title is-1">Dashboard</h1>

<div class="notification is-primary">All systems nominal</div>

<progress class="progress is-info" max="100" value="42"></progress>

<div>
  <button class="button is-primary">Refresh</button>
</div>
```

To get consistent spacing, wrap each chunk of content in a `.block` (or rely on the bottom margin `.block` applies):

```html
<div class="block">
  <h1 class="title is-1">Dashboard</h1>
</div>

<div class="block">
  <div class="notification is-primary">All systems nominal</div>
</div>

<div class="block">
  <progress class="progress is-info" max="100" value="42"></progress>
</div>

<div class="block">
  <button class="button is-primary">Refresh</button>
</div>
```

### Verifiable v1 facts this correction depends on (sourced from bulma.io)

- `.block` is listed in the Bulma Elements directory at `https://bulma.io/documentation/elements/` (alongside Box, Button, Content, Delete, Icon, Image, Notification, Progress, Table, Tag, Title) — confirmed present in v1.
- `.block` is a **spacing element**: it provides a bottom margin between itself and the next sibling, standardizing vertical rhythm across the page.
- `.box` (at `https://bulma.io/documentation/elements/box/`) is a **container** with a white surface, padding, rounded corners, and a subtle shadow — visually distinct from `.block`, which draws nothing.

## What the skill must therefore guarantee (minimal scope)

1. Name all 12 elements (`block`, `box`, `button`, `content`, `delete`, `icon`, `image`, `notification`, `progress`, `table`, `tag`, `title`) and route each to the right reference file.
2. Explicitly disambiguate `.block` (spacer) vs `.box` (surface) — the most common elements-level confusion.
3. Show the `.block` fix as a copy-paste pattern (closes the baseline failure above).
4. Cover the modifier ecosystem per element (colors, sizes, states) in `references/*.md`, NOT in the lean `SKILL.md`.
5. Hand off out-of-scope topics: spacing utility classes (`m*`/`p*`) → `bulma-helpers`; CSS-variable theming / dark mode → `bulma-customize`.

## References (verification)

- https://bulma.io/documentation/elements/ (Elements directory — confirms the 12-element list incl. Block, new in v1)
- https://bulma.io/documentation/elements/block/ (the `.block` spacer element)
- https://bulma.io/documentation/elements/box/ (the `.box` container — confirms it is a surface, NOT a spacer)
