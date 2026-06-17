# Bulma Elements — Box, Button, Notification

Reference catalog for the surface/action/message elements owned by `bulma-elements`. Source of truth: bulma.io element pages (URLs in the References section). This file holds the full class tables and the rich examples; the lean `SKILL.md` carries only headline classes and patterns.

---

## `box` — the bordered container

Source: https://bulma.io/documentation/elements/box/

A white, bordered container with padding, rounded corners, and a subtle box shadow. The "card-like" surface for grouping content. **`.box` is a container, not a spacer** — see `typography-and-content.md` for the `.block` vs `.box` distinction.

```html
<div class="box">
  <p class="title is-5">Account summary</p>
  <p>Balance, recent activity, and limits live here.</p>
</div>
```

`.box` has no size/color modifiers of its own — it's a neutral surface. Compose it with other elements (`title`, `content`, `tag`, etc.) to build what you need. For a richer composable surface with image/content/header/footer slots, use the `card` **component** (owned by `bulma-components`).

---

## `button` — the most modifier-rich element

Source: https://bulma.io/documentation/elements/button/

Works on `<button>`, `<a>`, `<input type="submit">`. Defaults to a neutral button; layer one modifier from each category (color, size, style, state).

```html
<button class="button is-primary is-medium is-rounded">Save</button>
<a class="button is-text">Cancel</a>
<input class="button is-success" type="submit" value="Submit">
```

### Colors

| Class | Color |
|---|---|
| `is-primary` | primary (turquoise by default) |
| `is-link` | link blue |
| `is-info` | info cyan |
| `is-success` | success green |
| `is-warning` | warning yellow |
| `is-danger` | danger red |
| (no color) | neutral (white/light) |

### Sizes

| Class | Size |
|---|---|
| `is-small` | small |
| `is-normal` | normal (small-ish) |
| (none) | default |
| `is-medium` | medium |
| `is-large` | large |

### Styles

| Class | Effect |
|---|---|
| `is-outlined` | transparent fill, colored border/text |
| `is-inverted` | swap text/bg colors (for use on colored backgrounds) |
| `is-rounded` | pill-shaped, fully rounded corners |
| `is-loading` | shows a spinner; non-interactive |
| `is-fullwidth` | 100% width |
| `is-text` / `is-ghost` | borderless text button |
| `is-static` | non-interactive, styled like a read-only value |

### States

| Class | Meaning |
|---|---|
| `is-hovered` | force the hover look |
| `is-focused` | force the focus ring |
| `is-active` | force the pressed look |
| `is-selected` | marks a selected button in a group |

### Disabled

Use the native `disabled` attribute (not a class) on `<button>`/`<input>`:

```html
<button class="button" disabled>Disabled</button>
```

### Icon buttons / icon + label

Wrap an icon and text (see `icon` in `typography-and-content.md`):

```html
<button class="button">
  <span class="icon"><i class="fas fa-save"></i></span>
  <span>Save</span>
</button>
```

### Button groups (`buttons`)

Wrap multiple buttons in `.buttons` for consistent spacing and alignment. Add modifiers on the wrapper:

| Wrapper modifier | Effect |
|---|---|
| `has-addons` | join buttons edge-to-edge (no gap between them) |
| `is-grouped` | group buttons left/right with auto margins |
| `is-centered` | horizontally center the group |
| `is-right` | align the group to the right |

```html
<!-- Joined addon group -->
<div class="buttons has-addons">
  <button class="button">Yes</button>
  <button class="button is-selected">Maybe</button>
  <button class="button">No</button>
</div>

<!-- Right-aligned group -->
<div class="buttons is-right">
  <button class="button">Cancel</button>
  <button class="button is-primary">Save</button>
</div>
```

---

## `notification` — bold message block

Source: https://bulma.io/documentation/elements/notification/

A full-width message block with an optional color, typically holding text and a `.delete` close button. Use for non-field messages (alerts, banners); for inline form validation use form states owned by `bulma-form`.

```html
<div class="notification is-danger">
  <button class="delete"></button>
  Something went wrong. Please try again.
</div>
```

### Colors

Same six color tokens as buttons: `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`. A notification without a color renders in the default light surface.

### `is-light` — pastel variant

Each color has a softer `is-light` version (light background, darker text) for less alarming messages:

```html
<div class="notification is-warning is-light">
  <button class="delete"></button>
  Heads up — your trial ends soon.
</div>
```

### Dismissing (small JS)

Bulma ships no JS. The `.delete` button is just a styled `×` — to actually remove the notification, attach a tiny listener:

```html
<div class="notification is-info">
  <button class="delete" onclick="this.parentElement.remove()"></button>
  Dismiss me.
</div>
```

> Bulma provides no JavaScript framework. The single-line `onclick` above is the minimal pattern; for app-level code, wire it up in your framework of choice. The same "Bulma ships no JS" principle applies to `bulma-components` (navbar, dropdown, modal, tabs).

---

## References (verification)

- https://bulma.io/documentation/elements/box/ — `.box` container
- https://bulma.io/documentation/elements/button/ — `.button` + full modifier ecosystem
- https://bulma.io/documentation/elements/notification/ — `.notification` colors / `is-light` / delete
