---
name: bulma-elements
description: Bulma elements. Use when the user asks about a Bulma block, box, button, content area, delete (x) icon, icon (with Font Awesome), image (figure/responsive), notification, progress bar, table, tag, or title and subtitle. Covers element classes, states, colors, sizes, and the modifier ecosystem for each. The block element is new in Bulma 1.0 and replaces spacing hacks between adjacent elements.
---

# Bulma Elements

Bulma **elements** are standalone interface pieces that need only a single CSS class to style a native HTML tag. This skill owns the 12 documented elements: `block`, `box`, `button`, `content`, `delete`, `icon`, `image`, `notification`, `progress`, `table`, `tag`, `title`/`subtitle`.

## When to use this skill

- The user names one of the 12 elements above: "Bulma button", "progress bar", "the × delete icon", "Bulma table", "title and subtitle", "a box", "a tag", "a notification".
- The user asks how to space stacked Bulma elements vertically without custom CSS (the `.block` element).
- The user wants an element's colors, sizes, or states ("a red button", "a loading button", "a large title", "an outlined button").
- The user is confused between `.block` (spacer) and `.box` (surface) — both live here and this skill disambiguates them.
- The user asks how to wrap a Font Awesome icon, or how to render a fixed-ratio responsive image.

**Out of scope — route to the sibling skill:**
- Spacing **utility classes** (`m*`, `p*`, `mt-4`, `mx-auto`, responsive `mx-*-mobile`) → `bulma-helpers`. (`.block` is an *element* owned here; the `m*`/`p*` utilities are owned by helpers.)
- Theming colors via `--bulma-*` CSS variables, dark mode, custom color palettes → `bulma-customize`.
- Composite components built from elements (card, navbar, modal, dropdown, message) → `bulma-components`.
- Form controls (`input`, `textarea`, `select`, `field`, `control`) → `bulma-form`.
- Columns/Grid layout for arranging elements side by side → `bulma-layout`.

## Key classes & concepts

- **`block` (new in v1)** — vertical-rhythm spacer. A bottom-margin wrapper that separates itself from the next sibling. Replaces `<br>` and ad-hoc `margin-top` hacks between stacked elements. Draws nothing visible.
- **`box`** — a container: white surface, padding, rounded corners, subtle shadow. NOT a spacer; the visual "card-like" box.
- **`button`** — the most modifier-rich element: colors (`is-primary`, `is-link`, `is-danger`, …), sizes (`is-small`, `is-medium`, `is-large`), styles (`is-outlined`, `is-inverted`, `is-rounded`, `is-loading`), states (`is-hovered`, `is-focused`, `is-active`, `is-static`), and grouping (`buttons` wrapper, `has-addons`, `is-grouped`).
- **`content`** — a typography wrapper for rendered HTML (markdown output, CMS blobs): styles `<h1>`–`<h6>`, `<ul>`, `<blockquote>`, `<table>` inside `.content` automatically.
- **`delete`** — the small `×` close button (`<button class="delete"></button>`); pairs with notifications, tags, messages.
- **`icon`** — a wrapper for icon fonts (Font Awesome): `<span class="icon"><i class="fas fa-home"></i></span>`. Sizes and colors track the surrounding text; load Font Awesome separately.
- **`image`** — a `<figure>`-based responsive image: `<figure class="image is-4by3"><img src="…"></figure>`. Fixed ratios (`is-16by9`, `is-4by3`, `is-1by1`, `is-square`, …) prevent layout shift; `is-rounded` (on the `<img>`) rounds the corners.
- **`notification`** — a bold message block with an optional color (`is-primary`, `is-danger`, …) and `is-light` pastel variant; typically holds a delete button.
- **`progress`** — styles the native `<progress>` element; colors and sizes apply; indeterminate when no `value` is set.
- **`table`** — styles `<table>`: modifiers `is-bordered`, `is-striped`, `is-narrow`, `is-hoverable`, `is-fullwidth`, plus a `table-container` wrapper for horizontal scroll.
- **`tag`** — a small label: colors, sizes (`is-normal`, `is-medium`, `is-large`), rounded (`is-rounded`), and add-on/delete variants (`tags has-addons`, `tag is-delete`).
- **`title` / `subtitle`** — headings: sizes `is-1` (largest) through `is-6`; add `is-spaced` to a title to preserve the gap before its subtitle (Bulma collapses that gap by default).

## Common patterns (copy-paste)

Standard vertical rhythm between stacked elements (the `.block` fix — see baseline-failure artifact):

```html
<div class="block"><h1 class="title is-1">Dashboard</h1></div>
<div class="block"><div class="notification is-primary">All systems nominal</div></div>
<div class="block"><progress class="progress is-info" max="100" value="42"></progress></div>
```

A button with the common modifier stack (color + size + style):

```html
<button class="button is-primary is-medium is-rounded is-loading">Save</button>
```

Buttons grouped in a row (the `buttons` wrapper; combine with `has-addons` to join them):

```html
<div class="buttons has-addons">
  <button class="button">Yes</button>
  <button class="button is-selected">Maybe</button>
  <button class="button">No</button>
</div>
```

A notification with a dismiss button:

```html
<div class="notification is-danger is-light">
  <button class="delete"></button>
  Something went wrong.
</div>
```

A fixed-ratio responsive image (the ratio goes on the `<figure>`, `is-rounded` on the `<img>`):

```html
<figure class="image is-4by3">
  <img class="is-rounded" src="/img/avatar.jpg" alt="avatar">
</figure>
```

A styled table inside a scrollable container:

```html
<div class="table-container">
  <table class="table is-bordered is-striped is-hoverable is-fullwidth">
    <thead><tr><th>Name</th><th>Role</th></tr></thead>
    <tbody><tr><td>Ava</td><td>Admin</td></tr></tbody>
  </table>
</div>
```

A title with a spaced subtitle:

```html
<p class="title is-3 is-spaced">Settings</p>
<p class="subtitle is-5">Manage your account</p>
```

## v1 gotchas

- **`.block` is new in Bulma 1.0** and is the standard vertical-rhythm spacer between adjacent elements. Use `<div class="block">…</div>` instead of `<br>` or inline `margin-top` hacks. (See `references/typography-and-content.md` and the baseline-failure artifact.)
- **`.block` ≠ `.box`.** `.block` is an invisible spacer (bottom margin); `.box` is a visible bordered/shadowed container. Both are elements in this skill — do not use one when you mean the other.
- **`icon` requires Font Awesome** (or another icon font) loaded separately. Bulma ships the `.icon` wrapper and sizing, not the glyphs themselves.
- **`image` uses fixed-ratio classes** on the `<figure>` (`is-16by9`, `is-4by3`, `is-1by1`, `is-square`, …) to reserve space and avoid layout shift; `is-rounded` is applied to the inner `<img>`.
- **`progress` styles the native `<progress>` element** — set `value`/`max` for determinate, omit `value` for indeterminate.
- **`title` + `subtitle` collapse their gap by default**; add `is-spaced` to the title to keep the breathing room between them.
- **Element theming is via Bulma's color tokens** (`is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`, …). Defining *new* colors or overriding `--bulma-*` variables is owned by `bulma-customize`, not here.

## References (load on demand)

Load these bundled files for full class catalogs, state/size tables, and richer per-element examples. Every file below is part of this skill:

- `references/typography-and-content.md` — Full catalogs for `title`/`subtitle` (sizes, `is-spaced`), `content` (typography wrapper), `block` (the new v1 spacer — what it replaces, when to use it, block vs box), `delete` (the × close icon), and `icon` (Font Awesome, sizes, colors, text wrappers).
- `references/box-button-notification.md` — Full catalogs for `box` (the bordered container), `button` (the entire modifier ecosystem: colors, sizes, styles `is-outlined`/`is-loading`/`is-inverted`/`is-rounded`, states, the `buttons` group wrapper, `has-addons`, `is-grouped`), and `notification` (colors, `is-light`, delete button, JS dismiss).
- `references/table-image-progress-tag.md` — Full catalogs for `table` (`is-bordered`/`is-striped`/`is-narrow`/`is-hoverable`/`is-fullwidth`, `table-container` scroll wrapper, row states), `image` (`<figure>` ratios `is-16by9`/`is-4by3`/`is-1by1`/`is-square`, `is-rounded`, fixed-square sizes), `progress` (colors, sizes, indeterminate), and `tag` (colors, sizes, `is-rounded`, `tags has-addons`, delete).
