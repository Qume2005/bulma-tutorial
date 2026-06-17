---
name: bulma-components
description: Bulma components. Use when the user asks about a Bulma breadcrumb, card, dropdown, menu (vertical sidebar nav), message, modal, navbar (top nav), pagination, panel, or tabs. Covers component structure, the small JavaScript needed for dropdown/modal/navbar/tabs interactivity, and responsive variants (e.g. navbar burger menu, mobile dropdown).
---

# Bulma Components

## When to use this skill

- The user asks how to build or structure a Bulma `navbar` (top nav), `breadcrumb`, `card`, `dropdown`, `menu` (vertical sidebar), `message`, `modal`, `pagination`, `panel`, or `tabs`.
- The user says their Bulma navbar burger, dropdown, or modal "doesn't work" / "won't open" / "clicking does nothing" — this is almost always the missing `is-active` toggle JS, which this skill owns.
- The user wants the responsive navbar (burger menu on mobile) or a responsive dropdown.
- The user wants the copy-paste vanilla-JS snippets that make Bulma's interactive components function (Bulma ships zero JavaScript).
- The user needs the full class catalog or a richer example for any of the 10 components.

**Out of scope — route to a sibling:**
- Theming component colors via `--bulma-*` CSS variables / Sass variables → `bulma-customize`.
- The general `is-*`/`has-*` modifier concept and the 8 theme colors → `bulma-fundamentals`.
- The `delete` element (used inside message/modal/notification) as a standalone element → `bulma-elements` (this skill only covers it where it's used in a component).
- Form controls placed inside navbars/panels → `bulma-form`.
- The `media` object and `columns`/`grid` used to compose card layouts → `bulma-layout`.

## Key classes & concepts

- **Bulma ships NO JavaScript.** Every interactive component (`navbar` burger, `dropdown`, `modal`) needs a few lines of vanilla JS you add. `is-active` is the **universal toggle class** — add it to show, remove it to hide.
- **`navbar`** — horizontal responsive nav. `navbar-brand` (logo + burger, always visible) → `navbar-menu` (`navbar-start`/`navbar-end`, hidden on mobile unless `is-active`). The burger must be the **last child** of `navbar-brand`.
- **`navbar-burger`** — the hamburger (3 `<span>`s). CSS-only animates to an X when it has `is-active`.
- **`dropdown`** — `dropdown` > `dropdown-trigger` > button, and `dropdown-menu` > `dropdown-content` > `dropdown-item`. Toggle `is-active` on the `.dropdown` container. `is-hoverable` = CSS-only hover variant (no JS).
- **`modal`** — `modal` > `modal-background` + (`modal-card` | `modal-content`) + close. Toggle `is-active` on `.modal`.
- **`breadcrumb`** — `breadcrumb` > `ul` > `li`. Separators via `has-arrow-separator`/`has-dot-separator`/etc. CSS-only.
- **`card`** — `card` with optional `card-image`/`card-header`/`card-content`/`card-footer`. CSS-only.
- **`menu`** — vertical sidebar nav: `menu` > `menu-label` + `menu-list` (`<ul>`, nestable). CSS-only.
- **`message`** — colored alert block: `message` > `message-header` (+ optional `.delete`) + `message-body`. Color/size modifiers. `.delete` dismiss needs a 1-line JS listener.
- **`pagination`** — `pagination` > `pagination-previous`/`pagination-next` + `pagination-list` of `pagination-link` (`is-current`). CSS-only.
- **`panel`** — composable sidebar/filter panel: `panel` > `panel-heading` + optional `panel-tabs` + `panel-block`s. CSS-only.
- **`tabs`** — `tabs` > `ul` > `li` (mark one `is-active`). Modifiers `is-boxed`/`is-toggle`/`is-centered`/`is-fullwidth`. Moving `is-active` to switch tabs needs JS (you also toggle your own content panels).

## Common patterns (copy-paste)

### Navbar burger toggle (the #1 "Bulma is broken" fix)

The HTML (burger is last child of `navbar-brand`):

```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="/">Brand</a>
    <a role="button" class="navbar-burger" aria-label="menu"
       aria-expanded="false" data-target="navbarBasicExample">
      <span></span><span></span><span></span>
    </a>
  </div>
  <div id="navbarBasicExample" class="navbar-menu">
    <div class="navbar-start"><a class="navbar-item">Home</a></div>
  </div>
</nav>
```

The JS (toggles `is-active` on **both** the burger and its target menu — this is the official snippet from bulma.io):

```javascript
document.addEventListener('DOMContentLoaded', () => {
  document.querySelectorAll('.navbar-burger').forEach(burger => {
    burger.addEventListener('click', () => {
      const target = document.getElementById(burger.dataset.target);
      burger.classList.toggle('is-active');
      target.classList.toggle('is-active');
    });
  });
});
```

### Dropdown toggle

```html
<div class="dropdown" id="dd">
  <div class="dropdown-trigger">
    <button class="button" aria-haspopup="true" aria-controls="dd-menu">
      <span>Choose</span>
    </button>
  </div>
  <div class="dropdown-menu" id="dd-menu" role="menu">
    <div class="dropdown-content">
      <a class="dropdown-item">A</a>
      <a class="dropdown-item">B</a>
    </div>
  </div>
</div>
<script>
  const dd = document.getElementById('dd');
  dd.querySelector('.dropdown-trigger .button').addEventListener('click', (e) => {
    e.stopPropagation();
    dd.classList.toggle('is-active');
  });
  document.addEventListener('click', () => dd.classList.remove('is-active'));
</script>
```

> No JS alternative: add `is-hoverable` to `.dropdown` for a CSS-only hover-open menu.

### Modal open/close

```html
<button class="button is-primary" id="open">Open</button>
<div class="modal" id="m">
  <div class="modal-background" data-close></div>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">Title</p>
      <button class="delete" data-close aria-label="close"></button>
    </header>
    <section class="modal-card-body">Body.</section>
  </div>
</div>
<script>
  const m = document.getElementById('m');
  const open  = () => m.classList.add('is-active');
  const close = () => m.classList.remove('is-active');
  document.getElementById('open').addEventListener('click', open);
  m.querySelectorAll('[data-close]').forEach(el => el.addEventListener('click', close));
  document.addEventListener('keydown', e => { if (e.key === 'Escape') close(); });
</script>
```

### Card (header + content + footer)

```html
<div class="card">
  <header class="card-header">
    <p class="card-header-title">Title</p>
  </header>
  <div class="card-content"><div class="content">Card body.</div></div>
  <footer class="card-footer">
    <a class="card-footer-item">Save</a>
    <a class="card-footer-item">Delete</a>
  </footer>
</div>
```

### Message (color + dismiss)

```html
<article class="message is-warning" id="msg">
  <div class="message-header">
    <p>Warning</p>
    <button class="delete" id="msgDel"></button>
  </div>
  <div class="message-body">Heads up!</div>
</article>
<script>
  document.getElementById('msgDel').addEventListener('click', () =>
    document.getElementById('msg').remove()
  );
</script>
```

## v1 gotchas

- **Bulma ships NO JS framework.** This is unchanged in v1 — interactivity is still a few lines of vanilla JS you add. Bootstrap bundles `bootstrap.js`; Bulma deliberately does not. `is-active` is the universal toggle for navbar/dropdown/modal/tabs/panel-block.
- **`is-active` toggles live on different elements per component:** navbar burger + navbar-menu (both), dropdown container, modal container, tab `<li>`, menu link, panel-block, pagination link. Getting the toggle target wrong is the most common bug.
- **`is-hoverable`** on a navbar `has-dropdown` item or a standalone `dropdown` makes it CSS-only (opens on hover, no JS) — useful when you don't need click logic.
- v1 exposes `--bulma-navbar-*`, `--bulma-dropdown-*`, `--bulma-modal-*`, `--bulma-card-*`, `--bulma-message-*`, `--bulma-panel-*` CSS variables for runtime theming (override them — see `bulma-customize`; do not recompile Sass for runtime color changes).
- Component **structure and class names are stable from 0.9 → 1.0** — the burger toggle snippet above works in both. The main v1 change is the theming surface (CSS variables), not the component markup.

## References (load on demand)

Load these bundled files when you need full catalogs, element tables, or richer/variant examples. Every file below is part of this skill:

- `references/navigation.md` — Full structure + element/modifier tables for `navbar` (brand, burger, menu, start/end, dropdown, transparent, fixed), `breadcrumb` (alignment, separators, sizes, icons), `pagination` (previous/next, list, link, current, ellipsis, rounded, alignment), `tabs` (boxed/toggle/fullwidth/centered/right, icons, minimal tab-switching JS), and `menu` (vertical sidebar nav: label, list, nested submenus).
- `references/overlays-and-dropdown.md` — Full structure + element tables for `dropdown` (trigger, menu, content, item, divider, `is-active`/`is-hoverable`/`is-up`/`is-right`, click-toggle JS with outside-click close), `modal` (background, card/content/image variants, `modal-card-head`/`-title`/`-body`/`-foot`, full open/close JS with Esc + background close), and `panel` (heading, tabs, blocks, icons).
- `references/card-and-message.md` — Full structure + variants for `card` (image, content, header/footer, header-title/icon, footer-item, horizontal/media layout) and `message` (header + body, all color modifiers, sizes, header-less variant, dismiss/delete JS).
