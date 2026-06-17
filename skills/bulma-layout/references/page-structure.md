# Page Structure Reference

> Part of the `bulma-layout` skill. This file covers the structural layout elements under Bulma's `layout/` section: `container`, `section`, `footer`, `hero`, `level`, `media-object`. For the two column/grid systems, see `columns.md` and `grid.md`.

**Bulma doc source:** https://bulma.io/documentation/layout/ (`container/`, `section/`, `footer/`, `hero/`, `level/`, `media-object/`)

## `container` — centered max-width wrapper

A simple wrapper that centers its content and caps the width on large viewports. Use it to prevent content from stretching edge-to-edge on wide screens.

```html
<div class="container">
  <!-- content centered, max-width capped -->
</div>
```

### Width modifiers
| Class | Max width |
|---|---|
| (default) | ~1024px (desktop breakpoint width) |
| `is-widescreen` | up to 1216px |
| `is-fullhd` | up to 1408px |
| `is-max-desktop` | cap at the desktop width (~1024px) even on larger screens |
| `is-max-widescreen` | cap at the widescreen width (~1216px) even on larger screens |
| `is-fluid` | full width minus fixed horizontal margins (no max) |

> By default the container only kicks in from the `desktop` breakpoint (≥ 1024px); below that it is full width. The `is-widescreen`/`is-fullhd` variants extend the cap upward.

## `section` — vertical padding band

Adds vertical padding to a horizontal slice of the page. The standard rhythm element between page regions.

```html
<section class="section">
  <div class="container">…</div>
</section>
```

### Sizes
| Class | Vertical padding |
|---|---|
| (default) | 3rem 1.5rem |
| `is-medium` | 9rem 4.5rem |
| `is-large` | 18rem 6rem |

## `footer` — page footer band

A full-width footer, typically dark with multi-column content (use `columns` inside).

```html
<footer class="footer">
  <div class="container">
    <div class="columns">
      <div class="column">…</div>
      <div class="column">…</div>
    </div>
  </div>
</footer>
```

The default footer has a light gray background (`$footer-background-color`); style it via Sass/CSS variables (see `bulma-customize`) or by overriding classes.

## `hero` — full-width banner

A full-width banner that can optionally cover the full viewport height. The structural backbone for landing/hero sections.

### Structure (three optional sub-regions)
| Class | Region |
|---|---|
| `hero-head` | Top band (navbar/logo often goes here) |
| `hero-body` | Centered main content (always present) |
| `hero-foot` | Bottom band (tabs/CTA row) |

```html
<section class="hero is-primary is-medium">
  <div class="hero-head">…</div>
  <div class="hero-body">
    <div class="container">
      <p class="title">Hero title</p>
      <p class="subtitle">Subtitle</p>
    </div>
  </div>
  <div class="hero-foot">…</div>
</section>
```

### Sizes
| Class | Effect |
|---|---|
| (default) | normal height (auto by content) |
| `is-small` | reduced vertical padding |
| `is-medium` | medium vertical padding |
| `is-large` | large vertical padding |
| `is-fullheight` | full viewport height |
| `is-fullheight-with-navbar` | full height minus a fixed navbar (use when a fixed navbar is present above) |

### Colors
Add a color modifier (`is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`, `is-light`, `is-dark`) to the hero to set its background + contrasting text. To override these colors, theme via CSS variables (`--bulma-*`) — see `bulma-customize`.

## `level` — horizontal flex row

A horizontal row of left/center/right items. Good for toolbars, stat rows, and anywhere you need a 3-part horizontal layout.

### Structure
| Class | Role |
|---|---|
| `level` | the flex container |
| `level-left` | left group |
| `level-right` | right group |
| `level-item` | a single item (can be used standalone to center items) |

```html
<div class="level">
  <div class="level-left">
    <div class="level-item"><strong>Left label</strong></div>
  </div>
  <div class="level-right">
    <div class="level-item">Right item</div>
    <div class="level-item">Another</div>
  </div>
</div>
```

### Modifiers
| Class | Effect |
|---|---|
| `is-mobile` | Keep the level horizontal on mobile (levels stack vertically on mobile by default). |
| `is-flex` | (with a standalone `level-item`) center a single item. |

## `media-object` — avatar + content

A horizontal pattern: avatar/icon on the left, content in the middle, optional action on the right. The backbone of comments, list items with thumbnails, and notifications.

### Structure
| Class | Role |
|---|---|
| `media` | the flex container |
| `media-left` | left slot (typically an avatar/image) |
| `media-content` | the main content (flex-grows to fill) |
| `media-right` | right slot (typically a close/action button) |

```html
<article class="media">
  <figure class="media-left">
    <p class="image is-64x64"><img src="avatar.png" alt=""></p>
  </figure>
  <div class="media-content">
    <div class="content"><p><strong>Author</strong> <small>@handle</small></p></div>
  </div>
  <div class="media-right">
    <button class="delete"></button>
  </div>
</article>
```

Media objects nest cleanly (a reply thread): put another `<article class="media">` inside `media-content`.

## Putting it together — a page skeleton

```html
<!-- HERO -->
<section class="hero is-primary is-medium">
  <div class="hero-body">
    <div class="container">
      <p class="title">Welcome</p>
      <p class="subtitle">A Bulma page skeleton</p>
    </div>
  </div>
</section>

<!-- MAIN SECTION: container + columns or grid -->
<section class="section">
  <div class="container">
    <div class="columns">
      <div class="column is-three-quarters">Main</div>
      <div class="column">Sidebar</div>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer class="footer">
  <div class="container"><p>© 2026</p></div>
</footer>
```

For the structural centering element, always reach for `container`; for the vertical band, reach for `section`; for a full-bleed banner, reach for `hero`. Combine these with `columns` (content-driven) or `grid` (fixed-cell) inside `container` to build out the body.

## References
- Container: https://bulma.io/documentation/layout/container/
- Section: https://bulma.io/documentation/layout/section/
- Footer: https://bulma.io/documentation/layout/footer/
- Hero: https://bulma.io/documentation/layout/hero/
- Level: https://bulma.io/documentation/layout/level/
- Media object: https://bulma.io/documentation/layout/media-object/
