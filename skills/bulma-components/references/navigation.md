# Reference — Navigation Components

> Part of `bulma-components`. Load this file when you need full catalogs and structure for `navbar`, `breadcrumb`, `pagination`, `tabs`, and `menu`. For the JS toggle snippets, see the SKILL.md "Common patterns" section (navbar burger) and `overlays-and-dropdown.md` (dropdown/modal).

All facts below are sourced from bulma.io (Bulma v1.0.x). Bulma is CSS-only — interactivity is a few lines of JS you add (see SKILL.md).

---

## Navbar

A responsive horizontal navigation bar. **The most complex Bulma component** — and the one that most often breaks for new users because of the burger-toggle JS requirement.

### Structure

```
nav.navbar
├── .navbar-brand              (always visible: logo + burger)
│   ├── .navbar-item           (logo / brand link)
│   └── .navbar-burger         (MUST be last child of navbar-brand; 3 <span>s)
└── #id.navbar-menu            (collapses on mobile, shows on desktop)
    ├── .navbar-start          (left-aligned items)
    │   └── .navbar-item
    │       └── .navbar-item.has-dropdown       (dropdown container)
    │           ├── .navbar-link                 (the trigger)
    │           └── .navbar-dropdown             (the menu)
    │               ├── .navbar-item
    │               └── .navbar-divider
    └── .navbar-end            (right-aligned items)
```

### Navbar container modifiers

| Class | Effect |
|---|---|
| `is-transparent` | Removes the navbar shadow/background for a transparent overlay look. |
| `is-fixed-top` | Pins the navbar to the top of the viewport (stays while scrolling). Add `<body class="has-navbar-fixed-top">` so content isn't hidden. |
| `is-fixed-bottom` | Pins to the bottom. Add `<body class="has-navbar-fixed-bottom">`. |
| `is-spaced` | Adds vertical padding (upper/lower padding on the navbar). |
| Color: `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`, `is-dark`, `is-light` | Sets navbar background + foreground. |

### Navbar item modifiers

| Class | Effect |
|---|---|
| `is-active` | Highlights the item (current page). |
| `is-tab` | Renders the item as a tab (bottom border on hover/active). |
| `has-dropdown` | Marks an item as a dropdown parent. |
| `has-dropdown-up` | Opens the dropdown upward. |
| `is-hoverable` | On a `has-dropdown` item: opens on hover instead of click (no JS needed for open, but CSS-only). |
| `is-expanded` | Item grows to fill available width. |

### Navbar dropdown modifiers

| Class | Effect |
|---|---|
| `is-right` | Aligns the dropdown menu to the right edge of its parent item. |
| `is-boxed` | Adds a box shadow + spacing to the dropdown menu (detached look). |

### Burger menu behavior (CSS + JS)

- The `.navbar-burger` is **hidden on desktop** and **visible on mobile** (below the `$navbar-breakpoint`, default `1024px` / desktop). This is pure CSS.
- The `.navbar-menu` is **visible on desktop** and **hidden on mobile** — *unless* it has `is-active`, which forces it visible on mobile.
- Therefore: on mobile, the menu is only shown when the burger toggle adds `is-active` to both the burger (so the lines become an X) and the menu (so it appears). **Both toggles are required.** See the SKILL.md "Common patterns" snippet.

### Full navbar example

```html
<nav class="navbar is-light" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="https://bulma.io">
      <img src="logo.png" width="112" height="28">
    </a>
    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false"
       data-target="navMenu">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div id="navMenu" class="navbar-menu">
    <div class="navbar-start">
      <a class="navbar-item">Home</a>
      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link">More</a>
        <div class="navbar-dropdown">
          <a class="navbar-item">About</a>
          <a class="navbar-item">Jobs</a>
          <hr class="navbar-divider">
          <a class="navbar-item">Contact</a>
        </div>
      </div>
    </div>
    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a class="button is-primary"><strong>Sign up</strong></a>
          <a class="button is-light">Log in</a>
        </div>
      </div>
    </div>
  </div>
</nav>
```

### v1 notes
- Navbar structure and classes are **stable from 0.9 → 1.0** — the burger toggle pattern is unchanged.
- `--bulma-navbar-*` CSS variables now drive the navbar's colors, dimensions, and the breakpoint (`--bulma-navbar-breakpoint`). Override these for runtime theming (see `bulma-customize`).

---

## Breadcrumb

A simple, CSS-only navigation trail. **No JS required** — it is purely presentational.

### Structure

```html
<nav class="breadcrumb" aria-label="breadcrumbs">
  <ul>
    <li><a href="#">Home</a></li>
    <li><a href="#">Documentation</a></li>
    <li class="is-active"><a href="#" aria-current="page">Breadcrumb</a></li>
  </ul>
</nav>
```

### Modifiers (applied to the `.breadcrumb` container)

| Class | Effect |
|---|---|
| `is-centered` | Centers the breadcrumb trail. |
| `is-right` | Right-aligns the breadcrumb trail. |
| `has-arrow-separator` | Uses `→` between items (default is `/`). |
| `has-bullet-separator` | Uses `•` between items. |
| `has-dot-separator` | Uses `⋅` between items. |
| `has-succeeds-separator` | Uses `›` between items. |
| `is-small` / `is-medium` / `is-large` | Size variants. |

### Icons in breadcrumbs
Add a Font Awesome `<span class="icon">` inside an `<li>`, before the `<a>`:

```html
<li>
  <a href="#">
    <span class="icon is-small"><i class="fas fa-home" aria-hidden="true"></i></span>
    <span>Home</span>
  </a>
</li>
```

---

## Pagination

A CSS-only pagination control. **No JS required** for appearance; wire up links/handlers as needed.

### Structure

```html
<nav class="pagination is-centered" role="navigation" aria-label="pagination">
  <a class="pagination-previous">Previous</a>
  <a class="pagination-next">Next page</a>
  <ul class="pagination-list">
    <li><a class="pagination-link" aria-label="Goto page 1">1</a></li>
    <li><span class="pagination-ellipsis">&hellip;</span></li>
    <li><a class="pagination-link is-current" aria-label="Page 46" aria-current="page">46</a></li>
  </ul>
</nav>
```

### Elements

| Class | Purpose |
|---|---|
| `pagination` | Container. |
| `pagination-previous` | The "Previous" button (left side). |
| `pagination-next` | The "Next" button (right side). |
| `pagination-list` | The `<ul>` of page links. |
| `pagination-link` | A single page number link (inside the list). |
| `pagination-link.is-current` | Marks the current page (highlighted). |
| `pagination-ellipsis` | The `…` gap indicator. |

### Modifiers (on `.pagination`)

| Class | Effect |
|---|---|
| `is-centered` | Centers the list between previous/next. |
| `is-right` | Right-aligns the list. |
| `is-rounded` | Makes the buttons/links rounded. |
| `is-small` / `is-medium` / `is-large` | Size variants. |

---

## Tabs

A CSS-only tab strip. The **active tab** is set by adding `is-active` to an `<li>`. Switching tabs (showing/hiding tab content panels) is **not provided by Bulma** — you add JS to move `is-active` between `<li>`s and toggle your own content panels.

### Structure

```html
<div class="tabs">
  <ul>
    <li class="is-active"><a>Pictures</a></li>
    <li><a>Music</a></li>
    <li><a>Videos</a></li>
    <li><a>Documents</a></li>
  </ul>
</div>
```

### Modifiers (on `.tabs`)

| Class | Effect |
|---|---|
| `is-centered` | Centers the tab row. |
| `is-right` | Right-aligns the tab row. |
| `is-small` / `is-medium` / `is-large` | Size variants. |
| `is-boxed` | Adds a box style (the tab looks like a folder tab). |
| `is-toggle` | Renders tabs as toggle buttons (rounded rectangles). |
| `is-toggle-rounded` | Like `is-toggle` but with fully rounded ends. |
| `is-fullwidth` | Tabs stretch to fill the container width evenly. |
| `is-vertical` | Stacks tabs vertically (v1). |

Modifiers **combine**: `tabs is-toggle is-fullwidth`, `tabs is-centered is-boxed`, etc.

### Icons on tabs

```html
<li class="is-active">
  <a>
    <span class="icon is-small"><i class="fas fa-image" aria-hidden="true"></i></span>
    <span>Pictures</span>
  </a>
</li>
```

### Minimal tab-switching JS (move `is-active`)

```html
<div class="tabs" id="myTabs">
  <ul>
    <li class="is-active"><a data-tab="pictures">Pictures</a></li>
    <li><a data-tab="music">Music</a></li>
  </ul>
</div>
<div id="pictures" class="tab-content">…</div>
<div id="music" class="tab-content" style="display:none">…</div>

<script>
  const tabs = document.querySelectorAll('#myTabs li');
  tabs.forEach(li => li.addEventListener('click', () => {
    tabs.forEach(t => t.classList.remove('is-active'));
    li.classList.add('is-active');
    // toggle your own content panels here using li.querySelector('a').dataset.tab
  }));
</script>
```

---

## Menu

A **vertical sidebar navigation** component. CSS-only; nesting is done with nested `<ul>`s. **No JS required** for the structure (you may add JS to expand/collapse branches).

### Structure

```html
<aside class="menu">
  <p class="menu-label">General</p>
  <ul class="menu-list">
    <li><a class="is-active">Dashboard</a></li>
    <li><a>Customers</a></li>
  </ul>

  <p class="menu-label">Administration</p>
  <ul class="menu-list">
    <li>
      <a>Manage Your Team</a>
      <ul>  <!-- nested list = submenu -->
        <li><a>Members</a></li>
        <li><a>Plugins</a></li>
        <li><a>Add a member</a></li>
      </ul>
    </li>
    <li><a>Invitations</a></li>
  </ul>
</aside>
```

### Elements

| Class | Purpose |
|---|---|
| `menu` | The container (usually an `<aside>`). |
| `menu-label` | A section heading (`<p>`). |
| `menu-list` | The `<ul>` of links. |
| (link).`is-active` | Highlights the current page link. |

### Rules
- A nested `<ul>` **inside a `<li>`** is rendered as an indented sub-list (no extra class needed) — this is how you create submenus.
- Add Font Awesome icons inside an `<a>` alongside text for icon+label items.

### v1 notes
- Menu is stable from 0.9 → 1.0. `--bulma-menu-*` CSS variables now drive colors/dimensions for runtime theming.

---

## Sources

- https://bulma.io/documentation/components/navbar/
- https://bulma.io/documentation/components/breadcrumb/
- https://bulma.io/documentation/components/pagination/
- https://bulma.io/documentation/components/tabs/
- https://bulma.io/documentation/components/menu/
