# Bulma Elements — Table, Image, Progress, Tag

Reference catalog for the data/media/label elements owned by `bulma-elements`. Source of truth: bulma.io element pages (URLs in the References section). This file holds the full class tables and richer examples; the lean `SKILL.md` carries only headline classes and patterns.

---

## `table` — styled HTML table

Source: https://bulma.io/documentation/elements/table/

Apply `.table` to a native `<table>`. Layer modifiers to control borders, striping, density, hover, and width.

```html
<table class="table is-bordered is-striped is-hoverable is-fullwidth">
  <thead><tr><th>Name</th><th>Role</th></tr></thead>
  <tbody>
    <tr><td>Ava</td><td>Admin</td></tr>
    <tr><td>Ben</td><td>Editor</td></tr>
  </tbody>
</table>
```

### Modifiers (apply to `<table>`)

| Class | Effect |
|---|---|
| `is-bordered` | add borders to all cells |
| `is-striped` | alternate row background colors |
| `is-narrow` | reduce cell padding for a denser table |
| `is-hoverable` | highlight a row on hover |
| `is-fullwidth` | stretch the table to 100% width |

### Row states (apply to `<tr>`)

| Class | Effect |
|---|---|
| `is-selected` | highlight the row (selected state) |
| `is-warning` / `is-success` / `is-danger` | color the row (contextual status) |

### Scrollable tables (`table-container`)

For wide tables on small screens, wrap in `table-container` to get horizontal scrolling instead of breaking the layout:

```html
<div class="table-container">
  <table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
    <!-- many columns -->
  </table>
</div>
```

---

## `image` — fixed-ratio responsive figure

Source: https://bulma.io/documentation/elements/image/

The `.image` element is a `<figure>` wrapper that holds an `<img>` at a fixed aspect ratio, reserving space so the page doesn't reflow when the image loads. The **ratio** goes on the `<figure>`; `is-rounded` (if wanted) goes on the `<img>`.

```html
<figure class="image is-4by3">
  <img src="/img/photo.jpg" alt="A photo">
</figure>
```

### Ratio classes (on `<figure class="image …">`)

| Class | Aspect ratio |
|---|---|
| `is-square` / `is-1by1` | 1:1 |
| `is-5by4` | 5:4 |
| `is-4by3` | 4:3 |
| `is-3by2` | 3:2 |
| `is-5by3` | 5:3 |
| `is-16by9` | 16:9 |
| `is-2by1` | 2:1 |
| `is-3by1` | 3:1 (wide banner) |
| `is-4by5` | 4:5 (portrait) |
| `is-3by4` | 3:4 (portrait) |
| `is-2by3` | 2:3 (portrait) |

(Additional ratio classes exist beyond this set; the above cover the common cases.)

### Fixed square sizes

For non-ratio fixed-square sizes (useful for avatars/thumbnails), use the fixed-size classes:

| Class | Size |
|---|---|
| `is-16x16` | 16px square |
| `is-24x24` | 24px square |
| `is-32x32` | 32px square |
| `is-48x48` | 48px square |
| `is-64x64` | 64px square |
| `is-96x96` | 96px square |
| `is-128x128` | 128px square |
| `is-256x256` | 256px square |
| `is-512x512` | 512px square |

### Rounded

Add `is-rounded` to the inner `<img>` to round the corners (fully circular when combined with a square ratio):

```html
<figure class="image is-128x128">
  <img class="is-rounded" src="/img/avatar.jpg" alt="avatar">
</figure>
```

### Retina / lazy

Use a 2x-resolution image inside a fixed-size `.image` for retina sharpness (Bulma constrains the display size; you supply the high-res source). For lazy loading, use the native `loading="lazy"` attribute on the `<img>` — Bulma does not add a dedicated lazy class.

---

## `progress` — styled native progress bar

Source: https://bulma.io/documentation/elements/progress/

Styles the native `<progress>` element. Set `value` and `max` for a determinate bar; omit `value` for an indeterminate (animated) bar.

```html
<!-- Determinate -->
<progress class="progress is-info" max="100" value="42"></progress>

<!-- Indeterminate -->
<progress class="progress is-small is-primary" max="100"></progress>
```

### Colors

Same six tokens: `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`. Default is neutral.

### Sizes

| Class | Size |
|---|---|
| `is-small` | small |
| (none) | default |
| `is-medium` | medium |
| `is-large` | large |

### Indeterminate

When you omit the `value` attribute (but keep `max`), Bulma renders the animated indeterminate state. Note: some browsers need the `value` attribute explicitly absent (not `value=""`).

---

## `tag` — small label

Source: https://bulma.io/documentation/elements/tag/

A small inline label. Works on `<span>`, `<a>`, `<button>`.

```html
<span class="tag is-success">Verified</span>
```

### Colors

Six tokens: `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger`. Default is the light neutral tag.

### Sizes

| Class | Size |
|---|---|
| `is-normal` | normal (the default small tag) |
| (none) | medium |
| `is-large` | large |

### Rounded

`is-rounded` makes the tag a pill:

```html
<span class="tag is-info is-rounded">New</span>
```

### Delete tag

`is-delete` turns the tag into a removable `×` chip (the tag itself becomes the close affordance):

```html
<a class="tag is-delete"></a>
```

### Tag groups (`tags`)

Wrap multiple tags in `.tags` for consistent spacing. Add `has-addons` to join tags edge-to-edge into a single compound tag:

```html
<!-- Spaced tag list -->
<div class="tags">
  <span class="tag is-info">JavaScript</span>
  <span class="tag is-success">CSS</span>
</div>

<!-- Joined addon tag (e.g. "Premium ✕") -->
<div class="tags has-addons">
  <span class="tag is-dark">Plan</span>
  <span class="tag is-success">Premium</span>
</div>
```

A tag can also pair with `.delete` to make a dismissible chip:

```html
<div class="tags has-addons">
  <span class="tag is-link">Feature</span>
  <a class="tag is-delete"></a>
</div>
```

---

## References (verification)

- https://bulma.io/documentation/elements/table/ — `.table` modifiers, `table-container`, row states
- https://bulma.io/documentation/elements/image/ — `.image` ratios, fixed sizes, `is-rounded`
- https://bulma.io/documentation/elements/progress/ — `.progress` colors/sizes, indeterminate
- https://bulma.io/documentation/elements/tag/ — `.tag` colors/sizes/round, `tags has-addons`, delete
