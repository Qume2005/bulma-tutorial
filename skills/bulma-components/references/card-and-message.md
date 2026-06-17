# Reference — Card and Message

> Part of `bulma-components`. Load this file for full structure, catalogs, and variants for `card` and `message`. Both are **CSS-only** (no JS needed), except message dismiss which uses a tiny JS snippet to remove the element on click of its `.delete` button.

All facts below are sourced from bulma.io (Bulma v1.0.x).

---

## Card

A flexible content container with optional header, image, content, and footer. **CSS-only.**

### Structure — Card with header, image, content, footer

```html
<div class="card">
  <div class="card-image">
    <figure class="image is-4by3">
      <img src="placeholder.jpg" alt="Placeholder image">
    </figure>
  </div>

  <div class="card-content">
    <div class="media">
      <div class="media-left">
        <figure class="image is-48x48">
          <img src="avatar.png" alt="Avatar">
        </figure>
      </div>
      <div class="media-content">
        <p class="title is-4">John Smith</p>
        <p class="subtitle is-6">@johnsmith</p>
      </div>
    </div>
    <div class="content">
      Lorem ipsum dolor sit amet. <a href="#">@bulmaio</a>.
      <a href="#">#css</a> <a href="#">#responsive</a>
      <br>
      <time datetime="2016-1-1">11:09 PM - 1 Jan 2016</time>
    </div>
  </div>
</div>
```

### Structure — Card with header + footer

```html
<div class="card">
  <header class="card-header">
    <p class="card-header-title">Component</p>
    <button class="card-header-icon" aria-label="more options">
      <span class="icon"><i class="fas fa-angle-down" aria-hidden="true"></i></span>
    </button>
  </header>
  <div class="card-content">
    <div class="content">Card content here.</div>
  </div>
  <footer class="card-footer">
    <a href="#" class="card-footer-item">Save</a>
    <a href="#" class="card-footer-item">Edit</a>
    <a href="#" class="card-footer-item">Delete</a>
  </footer>
</div>
```

### Elements

| Class | Purpose |
|---|---|
| `card` | The container. |
| `card-image` | Wraps a `figure.image` (the card's image area). |
| `card-content` | The main padded content area. |
| `card-header` | A header bar (title + optional icon button). |
| `card-header-title` | Left-aligned title text in the header. |
| `card-header-icon` | Right-aligned icon button in the header. |
| `card-footer` | A footer bar. |
| `card-footer-item` | An evenly-spaced item (usually `<a>`) inside the footer. |

### Horizontal card

Add `media` to the `card` container, then place `media-left` (image) and `media-content` side by side:

```html
<div class="card">
  <div class="card-content">
    <div class="media">
      <div class="media-left">
        <figure class="image is-48x48"><img src="avatar.png" alt=""></figure>
      </div>
      <div class="media-content">
        <p class="title is-4">Title</p>
        <p class="subtitle is-6">Subtitle</p>
      </div>
    </div>
  </div>
</div>
```

> For a true full-horizontal card (image on the whole left, content on the right, stacked on mobile), Bulma docs show wrapping the image and content in a `columns` layout inside the card. See `bulma-layout` for the `columns` system.

### Notes
- The `card` itself has **no color or size modifiers** — style the content (titles, buttons) inside it. Theme via `--bulma-card-*` variables (see `bulma-customize`).
- The `media`/`media-left`/`media-content`/`media-right` classes are the **media object** (owned by `bulma-layout`), reused here for avatar+text rows.

### v1 notes
- Card is stable from 0.9 → 1.0. `--bulma-card-*` variables drive the shadow/color/radius for runtime theming.

---

## Message

A colored block with an optional header bar and body, used for alerts/notices. **CSS-only** except for the optional dismiss (delete) button, which you wire up with a one-line JS listener.

### Structure

```html
<article class="message is-dark">
  <div class="message-header">
    <p>Dark</p>
    <button class="delete" aria-label="delete"></button>
  </div>
  <div class="message-body">
    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
  </div>
</article>
```

### Elements

| Class | Purpose |
|---|---|
| `message` | The container (`<article>`). |
| `message-header` | The colored header bar (title + optional delete button). |
| `message-body` | The content area (lighter background). |
| `delete` (inside header) | The × close button (the shared delete element; see `bulma-elements`). |

### Color modifiers (on `.message`)

All 8 theme colors plus dark/light variants:

`is-dark`, `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`, `is-light` (light-tone variant of any color, e.g. `is-warning is-light`).

The header takes the strong color; the body takes a light tint of that color.

### Size modifiers (on `.message`)

`is-small`, (default), `is-medium`, `is-large`.

### Header-less message

You can omit the `message-header` entirely and use just a colored `message` + `message-body`:

```html
<article class="message is-success">
  <div class="message-body">
    Saved successfully.
  </div>
</article>
```

### Dismiss (delete button) JS

The `.delete` button does nothing by default — add a one-line listener to remove the message on click:

```html
<article class="message is-warning" id="dismissibleMsg">
  <div class="message-header">
    <p>Warning</p>
    <button class="delete" id="dismissBtn"></button>
  </div>
  <div class="message-body">You can dismiss me.</div>
</article>

<script>
  document.getElementById('dismissBtn').addEventListener('click', () => {
    document.getElementById('dismissibleMsg').remove();
  });
</script>
```

### v1 notes
- Message is stable from 0.9 → 1.0. `--bulma-message-*` variables drive the header/body colors and the `is-light` tint.
- The `delete` button is the **shared delete element** (owned by `bulma-elements`) — it appears in `message`, `notification`, `modal-card-head`, and `tag`.

---

## Sources

- https://bulma.io/documentation/components/card/
- https://bulma.io/documentation/components/message/
