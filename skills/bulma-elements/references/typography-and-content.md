# Bulma Elements — Typography & Content (`title`, `subtitle`, `content`, `block`, `delete`, `icon`)

Reference catalog for the text/spacing/icon elements owned by `bulma-elements`. Source of truth: bulma.io element pages (URLs in the References section). This file holds the full class tables; the lean `SKILL.md` carries only headline classes and patterns.

---

## `block` — the v1 vertical-rhythm spacer

**New in Bulma 1.0.** Source: https://bulma.io/documentation/elements/block/

The `.block` element provides consistent spacing between itself and the next sibling via a bottom margin. It is the standard, semantic replacement for legacy `<br>` / inline `margin-top` hacks between stacked elements. `.block` draws no visible surface — it is purely a rhythm tool.

### What `.block` replaces

| Anti-pattern (0.9-era / ad-hoc) | v1-correct |
|---|---|
| `<br>` between stacked components | wrap each in `<div class="block">` |
| `style="margin-top: 1.5rem;"` | `<div class="block">` |
| empty `<div class="mb-4"></div>` shims | `<div class="block">` |

### Usage

```html
<div class="block">
  <h1 class="title is-1">Page heading</h1>
</div>

<div class="block">
  <div class="notification is-primary">First message</div>
</div>

<div class="block">
  <div class="notification is-info">Second message</div>
</div>
```

Each `.block` gets a uniform bottom margin, so the three blocks above sit on a consistent vertical grid regardless of what is inside them.

### `.block` vs `.box` — the most common confusion

These two elements have similar names and completely different jobs. This skill owns both; do not mix them up.

| Element | What it is | Visible? | Purpose |
|---|---|---|---|
| `.block` | a **spacer** (bottom margin) | No — draws nothing | Vertical rhythm between stacked siblings |
| `.box` | a **container** (white surface, padding, radius, shadow) | Yes — a card-like panel | Visually group content in a panel |

Using `.box` to space things litters the page with unwanted shadows and surfaces; using `.block` where you wanted a panel leaves you with invisible content. The fix is to read the column on the right.

> Out of scope: responsive spacing **utilities** like `m*`/`p*`/`mt-4`/`mx-auto` are owned by `bulma-helpers`, not here. `.block` is an *element*; the `m*`/`p*` classes are *helpers*.

---

## `title` / `subtitle` — headings

Source: https://bulma.io/documentation/elements/title/

Apply to any tag (`<h1>`, `<p>`, `<div>`). Sizes are `is-1` (largest) through `is-6` (smallest).

```html
<h1 class="title is-1">Title</h1>
<h2 class="title is-2">Section</h2>
<p class="subtitle is-5">Subtitle copy</p>
```

### Sizes

| Class | Use |
|---|---|
| `is-1` | page title (largest) |
| `is-2` … `is-5` | section / sub-section headings |
| `is-6` | smallest heading |

### Title + subtitle spacing (`is-spaced`)

By default Bulma **removes the gap** between a title and the subtitle that immediately follows it, treating them as a single heading cluster. To keep the normal breathing room, add `is-spaced` to the **title**:

```html
<p class="title is-3 is-spaced">Settings</p>
<p class="subtitle is-5">Manage your account</p>
```

---

## `content` — typography wrapper for rendered HTML

Source: https://bulma.io/documentation/elements/content/

`.content` is a wrapper that applies consistent typography to a chunk of rendered HTML — typically markdown output, CMS HTML, or any blob where you don't control the tags. Wrap it once and Bulma styles the `<h1>`–`<h6>`, `<ul>`/`<ol>`, `<blockquote>`, `<p>`, `<table>`, `<pre>`/`<code>`, `<img>` inside.

```html
<div class="content">
  <h1>Rendered markdown</h1>
  <p>This paragraph, the list below, and the blockquote are all styled by .content.</p>
  <ul>
    <li>Bullet one</li>
    <li>Bullet two</li>
  </ul>
  <blockquote>A pulled quote.</blockquote>
</div>
```

### Sizes

`.content` accepts the size modifiers `is-small`, `is-medium`, `is-large` to scale all the inner typography uniformly. Without a size, it renders at the default body scale.

---

## `delete` — the × close icon

Source: https://bulma.io/documentation/elements/delete/

A self-closing close button that renders a small `×`. Pairs with dismissible elements: `notification`, `tag` (delete variant), and the `message` component (owned by `bulma-components`).

```html
<button class="delete"></button>
<a class="delete"></a>
```

### Sizes

| Class | Result |
|---|---|
| (none) | default |
| `is-small` | small |
| `is-medium` | medium |
| `is-large` | large |

Typical pairing with a notification:

```html
<div class="notification is-info">
  <button class="delete"></button>
  You can dismiss this.
</div>
```

---

## `icon` — wrapper for icon fonts (Font Awesome)

Source: https://bulma.io/documentation/elements/icon/

`.icon` is a **wrapper** that sizes and positions an icon-font glyph (Font Awesome is the canonical pairing) and aligns it with surrounding text. Bulma provides the wrapper and the sizing/alignment logic — **you must load Font Awesome (or another icon font) yourself.** Bulma does not ship the glyphs.

```html
<span class="icon">
  <i class="fas fa-home"></i>
</span>

<span class="icon-text">
  <span class="icon"><i class="fas fa-home"></i></span>
  <span>Home</span>
</span>
```

### Sizes

`.icon` picks up color from the surrounding text and sizes from these modifiers (which set the icon's box, not the text):

| Class | Size |
|---|---|
| (none) | default (1.5em box) |
| `is-small` | small |
| `is-medium` | medium |
| `is-large` | large |

### Colors

Icons inherit text color. Combine with color helpers (`has-text-danger`, `has-text-success`, … — owned by `bulma-helpers`) or with colored parent elements:

```html
<span class="icon has-text-danger"><i class="fas fa-triangle-exclamation"></i></span>
```

### `icon-text` — icon next to text

The `icon-text` wrapper keeps an icon and its label aligned and wrapping together:

```html
<span class="icon-text">
  <span class="icon"><i class="fas fa-check"></i></span>
  <span>Saved successfully</span>
</span>
```

---

## References (verification)

- https://bulma.io/documentation/elements/block/ — `.block` spacer (new in v1)
- https://bulma.io/documentation/elements/title/ — `title` / `subtitle`, `is-spaced`
- https://bulma.io/documentation/elements/content/ — `.content` typography wrapper
- https://bulma.io/documentation/elements/delete/ — `.delete` × close button
- https://bulma.io/documentation/elements/icon/ — `.icon` / `.icon-text` (Font Awesome)
- https://bulma.io/documentation/elements/ — Elements directory (confirms the 12-element list incl. Block, new in v1)
