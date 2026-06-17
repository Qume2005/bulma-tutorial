# Reference — Dropdown, Modal, Panel

> Part of `bulma-components`. Load this file for full structure, catalogs, and the JS toggle snippets for `dropdown` and `modal` (the two overlay components), plus the `panel` component. For the navbar burger JS, see SKILL.md "Common patterns".

All facts below are sourced from bulma.io (Bulma v1.0.x). Bulma is **CSS-only** — the `is-active` toggle for dropdown and modal is JavaScript you write.

---

## Dropdown

An interactive menu that drops down from a trigger. **Requires JS** to toggle `is-active` on the `.dropdown` container (unless you use `is-hoverable` for a CSS-only hover variant).

### Structure

```html
<div class="dropdown">
  <div class="dropdown-trigger">
    <button class="button" aria-haspopup="true" aria-controls="dropdown-menu">
      <span>Dropdown</span>
      <span class="icon is-small"><i class="fas fa-angle-down" aria-hidden="true"></i></span>
    </button>
  </div>
  <div class="dropdown-menu" id="dropdown-menu" role="menu">
    <div class="dropdown-content">
      <a href="#" class="dropdown-item">Overview</a>
      <a href="#" class="dropdown-item">Modifiers</a>
      <a href="#" class="dropdown-item">Grid</a>
      <hr class="dropdown-divider">
      <a href="#" class="dropdown-item">More</a>
    </div>
  </div>
</div>
```

### Elements

| Class | Purpose |
|---|---|
| `dropdown` | The container. **`is-active` is toggled HERE** (not on the menu). |
| `dropdown-trigger` | Wraps the trigger button. |
| `dropdown-menu` | The positioned menu container (toggled by `is-active` on parent). |
| `dropdown-content` | Inner content box (white, shadowed). |
| `dropdown-item` | A clickable item. |
| `dropdown-divider` | A horizontal separator (`<hr>`). |
| `dropdown-item.is-active` | Highlights the item (selected state). |

### Modifiers (on `.dropdown`)

| Class | Effect |
|---|---|
| `is-active` | Shows the dropdown menu (the universal toggle). |
| `is-hoverable` | Opens the menu on hover instead of click — **CSS-only, no JS needed**. |
| `is-up` | Opens the menu **upward** (above the trigger). |
| `is-right` | Aligns the menu to the **right** edge of the trigger. |

### Minimal dropdown toggle JS (click-trigger variant)

```html
<div class="dropdown" id="myDropdown">
  <div class="dropdown-trigger">
    <button class="button" aria-haspopup="true" aria-controls="dropdown-menu3">
      <span>Toggle</span>
    </button>
  </div>
  <div class="dropdown-menu" id="dropdown-menu3" role="menu">
    <div class="dropdown-content">
      <a class="dropdown-item">Item A</a>
      <a class="dropdown-item">Item B</a>
    </div>
  </div>
</div>

<script>
  const dropdown = document.getElementById('myDropdown');
  const trigger  = dropdown.querySelector('.dropdown-trigger .button');

  trigger.addEventListener('click', (e) => {
    e.stopPropagation();
    dropdown.classList.toggle('is-active');
  });

  // close when clicking an item
  dropdown.querySelectorAll('.dropdown-item').forEach(item =>
    item.addEventListener('click', () => dropdown.classList.remove('is-active'))
  );

  // close when clicking outside
  document.addEventListener('click', () => dropdown.classList.remove('is-active'));
</script>
```

**Tip:** If you only need hover-to-open behavior and no click logic, skip the JS entirely and use `is-hoverable` on the `.dropdown`. The CSS handles open/close on hover/focus.

### v1 notes
- Dropdown is stable from 0.9 → 1.0. `--bulma-dropdown-*` variables drive dimensions/colors.

---

## Modal

A full-screen overlay dialog. **Requires JS** to add/remove `is-active` on the `.modal` container. Three structural variants: **modal card**, **modal content**, and **image modal**.

### Structure — Modal card (most common)

```html
<div class="modal">
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Modal title</p>
      <button class="delete" aria-label="close"></button>
    </header>
    <section class="modal-card-body">
      <!-- content -->
    </section>
    <footer class="modal-card-foot">
      <button class="button is-success">Save changes</button>
      <button class="button">Cancel</button>
    </footer>
  </div>
</div>
```

### Structure — Modal content (free-form)

```html
<div class="modal">
  <div class="modal-background"></div>
  <div class="modal-content">
    <!-- any HTML content -->
  </div>
  <button class="modal-close is-large" aria-label="close"></button>
</div>
```

### Structure — Image modal

```html
<div class="modal">
  <div class="modal-background"></div>
  <div class="modal-content">
    <p class="image">
      <img src="big-image.png" alt="">
    </p>
  </div>
  <button class="modal-close is-large" aria-label="close"></button>
</div>
```

### Elements

| Class | Purpose |
|---|---|
| `modal` | The container. **`is-active` is toggled HERE**. |
| `modal-background` | The semi-transparent backdrop (click to close). |
| `modal-content` | Centered free-form content box (max-width constrained). |
| `modal-card` | A structured card: head/body/foot. |
| `modal-card-head` | Header bar (title + close button). |
| `modal-card-title` | The title text (`<p>`). |
| `modal-card-body` | The body content (`<section>`). |
| `modal-card-foot` | Footer bar (actions). |
| `modal-close` | The close (×) button — use `.delete` inside a card head, or `.modal-close` for content/image variants. |
| `modal.is-active` | Shows the modal (the universal toggle). |

### How it works
- Without `is-active`, the `.modal` is `display: none` and invisible.
- Adding `is-active` makes it a fixed full-screen overlay with the backdrop + centered content.
- Removing `is-active` hides it again.

### Minimal modal open/close JS

```html
<!-- trigger -->
<button class="button is-primary" id="openModalBtn">Open modal</button>

<!-- modal -->
<div class="modal" id="myModal">
  <div class="modal-background"></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Hello</p>
      <button class="delete" aria-label="close" data-close></button>
    </header>
    <section class="modal-card-body">
      Modal content goes here.
    </section>
    <footer class="modal-card-foot">
      <button class="button is-success">Save</button>
      <button class="button" data-close>Cancel</button>
    </footer>
  </div>
</div>

<script>
  const modal = document.getElementById('myModal');
  const open  = () => modal.classList.add('is-active');
  const close = () => modal.classList.remove('is-active');

  document.getElementById('openModalBtn').addEventListener('click', open);

  // close on any [data-close] element, on the background, and on Esc
  modal.querySelectorAll('[data-close]').forEach(el =>
    el.addEventListener('click', close)
  );
  modal.querySelector('.modal-background').addEventListener('click', close);
  document.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') close();
  });
</script>
```

### v1 notes
- Modal is stable from 0.9 → 1.0. The `is-active` toggle pattern is unchanged.
- Some v1 docs/contributors show a native `<dialog>` HTML element as an alternative to the classic Bulma modal — the classic `.modal` + `is-active` approach is still the documented Bulma way.

---

## Panel

A container that composes a heading, optional tab strip, and a stack of blocks. Often used for filter/sidebar panels. **CSS-only** (no JS needed for structure; tab switching reuses the same `is-active` pattern as tabs if you want interactivity).

### Structure

```html
<nav class="panel">
  <p class="panel-heading">Repositories</p>

  <div class="panel-block">
    <p class="control has-icons-left">
      <input class="input" type="text" placeholder="Search">
      <span class="icon is-left"><i class="fas fa-search" aria-hidden="true"></i></span>
    </p>
  </div>

  <p class="panel-tabs">
    <a class="is-active">All</a>
    <a>Public</a>
    <a>Private</a>
    <a>Sources</a>
    <a>Forks</a>
  </p>

  <a class="panel-block is-active">
    <span class="panel-icon"><i class="fas fa-book" aria-hidden="true"></i></span>
    bulma
  </a>
  <a class="panel-block">
    <span class="panel-icon"><i class="fas fa-book" aria-hidden="true"></i></span>
    marksheet
  </a>

  <div class="panel-block">
    <button class="button is-link is-outlined is-fullwidth">
      Reset all filters
    </button>
  </div>
</nav>
```

### Elements

| Class | Purpose |
|---|---|
| `panel` | The container (`<nav>` is conventional). |
| `panel-heading` | A single heading bar at the top. |
| `panel-tabs` | A horizontal tab strip (each tab is an `<a>`; mark one `is-active`). |
| `panel-block` | A horizontal row container — holds controls, links, buttons, or any content. Stacks vertically. |
| `panel-block.is-active` | Highlights the block (selected filter/item). |
| `panel-icon` | An icon wrapper inside a block. |
| `panel-block` can also wrap a `control` with inputs (for filter/search bars). |

### Rules
- Blocks stack vertically with consistent spacing and hover styling.
- Use `panel-block` on an `<a>` to make a clickable row; on a `<div>`/`<label>` for controls.
- The panel has **no size/color modifiers of its own** — style children (buttons, inputs) individually, or theme the `--bulma-panel-*` CSS variables (see `bulma-customize`).

### v1 notes
- Panel is stable from 0.9 → 1.0. `--bulma-panel-*` variables drive colors/dimensions for runtime theming.

---

## Sources

- https://bulma.io/documentation/components/dropdown/
- https://bulma.io/documentation/components/modal/
- https://bulma.io/documentation/components/panel/
