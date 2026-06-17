# Form Controls — Full Catalog

> Reference file for the `bulma-form` skill. Per-control classes, states, sizes, colors, and grouping modifiers. Sourced from bulma.io (see Sources at the bottom). Bulma does NOT auto-style bare native elements — every styled control needs its Bulma class AND the `field` > `control` wrapper structure.

## The backbone: `field` > `control` > control-type

| Layer | Class | Role |
|---|---|---|
| `field` | `<div class="field">` | Vertical spacing between fields; hosts grouping modifiers (`has-addons`, `is-grouped`, `is-horizontal`). |
| `control` | `<div class="control">` | Hosts icons (`has-icons-left/right`), the loading spinner (`is-loading`), and is the unit that addons attach. |
| control-type | `input` / `textarea` / `select` wrapper / `checkbox` / `radio` / `file` | The styled native control. THIS is where you add `is-danger`, `is-large`, etc. |

Minimum viable field:

```html
<div class="field">
  <label class="label">Label</label>
  <div class="control">
    <input class="input" type="text" placeholder="Text input">
  </div>
</div>
```

`field` also accepts a help line and multiple controls. A `label` is optional but the Bulma-styled label class is `label` (plus `help` for the small grey caption, and `field-label`/`field-body` for horizontal layouts).

---

## `field` modifiers (grouping + layout)

| Modifier | What it does |
|---|---|
| `field has-addons` | Fuses child controls together — no gap, shared border radius. Use for attached input + button, input + static prefix. |
| `field has-addons has-addons-centered` | Addon row, centered. |
| `field has-addons has-addons-right` | Addon row, right-aligned. |
| `field is-grouped` | Keeps controls as separate items with spacing (vs fused). Use for button rows. |
| `field is-grouped is-grouped-centered` | Grouped row, centered. |
| `field is-grouped is-grouped-right` | Grouped row, right-aligned. |
| `field is-horizontal` | Lays the label and field body on one line (requires `field-label` + `field-body` children). |

`has-addons` vs `is-grouped` — the rule of thumb:
- Controls that are ONE logical input (a search box + its submit button, a URL + a protocol dropdown) → `has-addons`.
- Controls that are SEPARATE actions (Save, Cancel) → `is-grouped`.

Horizontal field anatomy:

```html
<div class="field is-horizontal">
  <div class="field-label">
    <label class="label">Normal field label</label>
  </div>
  <div class="field-body">
    <div class="field">
      <div class="control">
        <input class="input" type="text" placeholder="Field body">
      </div>
    </div>
  </div>
</div>
```

`field-label` accepts sizes: `is-normal`, `is-small`, `is-medium`, `is-large` — match it to the field body size.

---

## `control` modifiers

| Modifier | What it does |
|---|---|
| `control is-loading` | Shows a spinner inside the control (the native control stays interactive; the spinner overlays). Put this on `control`, never on `input`. |
| `control has-icons-left` | Reserves space for a left icon (e.g. user glyph). Pair with an `<span class="icon is-small is-left">`. |
| `control has-icons-right` | Reserves space for a right icon (e.g. validation check / warning). Pair with an `<span class="icon is-small is-right">`. |
| `control is-expanded` | Makes the control fill available width inside a `has-addons` / `is-grouped` row. |

Icon sizing rule: the `icon` span must match the input size — `icon is-small` for default/small inputs, `icon` (no size class) for `is-medium` inputs, `icon is-large` for `is-large` inputs.

---

## `input`

The styled `<input>`. Apply `class="input"` to a native `<input type="text|email|password|number|...">`.

### States / colors
| Class | Effect |
|---|---|
| `input is-loading` | (Not valid here — `is-loading` lives on the `control`.) |
| `input is-focused` | Visual focus state (normally you let `:focus` handle this). |
| `input is-hovered` | Visual hover state. |
| `input is-primary` / `is-link` / `is-info` / `is-success` / `is-warning` / `is-danger` | Recolors border + focus ring with the named theme color. |
| `input is-rounded` | Fully rounded corners. |
| `input is-static` | Strips border + background — for read-only display inside addons. |

### Validation (the system)
Bulma couples the input's state class with a sibling `help` paragraph:

```html
<div class="field">
  <label class="label">Email</label>
  <div class="control has-icons-left has-icons-right">
    <input class="input is-danger" type="email" placeholder="Email input" value="hello@">
    <span class="icon is-small is-left"><i class="fas fa-envelope"></i></span>
    <span class="icon is-small is-right"><i class="fas fa-exclamation-triangle"></i></span>
  </div>
  <p class="help is-danger">This email is invalid</p>
</div>
```

`help` is the small caption class. It accepts the same color states (`is-danger`, `is-success`, `is-info`, `is-warning`) so the message color always matches the input border. Do not inline-style the border or text color — use this pair.

### Sizes
`is-small`, `is-medium`, `is-large`. The `icon` span must match (see icon sizing rule above).

---

## `textarea`

Apply `class="textarea"` to a native `<textarea>`. Accepts the same color states, `is-hovered`/`is-focused`, and the same sizes. Rows are controlled by the native `rows` attribute.

```html
<div class="field">
  <label class="label">Message</label>
  <div class="control">
    <textarea class="textarea" placeholder="Textarea"></textarea>
  </div>
</div>
```

---

## `select`

A `<div class="select">` WRAPPER around a native `<select>`. Putting the class directly on the `<select>` does not work — the wrapper owns the arrow glyph, sizing, and state styling.

```html
<div class="control">
  <div class="select">
    <select>
      <option>Select dropdown</option>
      <option>With options</option>
    </select>
  </div>
</div>
```

| Class (on the `.select` wrapper) | Effect |
|---|---|
| `is-multiple` | Multiple-select mode; the inner `<select>` should have `multiple` and a fixed height. |
| `is-rounded` | Rounded corners on the wrapper. |
| `is-loading` | Spinner inside the select wrapper. |
| `is-small` / `is-medium` / `is-large` | Sizes. |
| `is-primary`...`is-danger` | Color states (same palette as input). |

`is-loading` can be on EITHER the `.select` wrapper OR the `.control`; for selects the canonical spot is the `.select` wrapper.

---

## `checkbox`

A `<label class="checkbox">` wrapping `<input type="checkbox">`. Intentionally unstyled to preserve cross-browser rendering.

```html
<div class="field">
  <label class="checkbox">
    <input type="checkbox"> I agree to the terms
  </label>
</div>
```

Multiple checkboxes go in a `field` (optionally with `label` header); they stack via the field spacing.

---

## `radio`

A `<label class="radio">` wrapping `<input type="radio">`. Pre-check one with the native `checked` attribute.

```html
<div class="field">
  <label class="radio">
    <input type="radio" name="question" checked> Yes
  </label>
  <label class="radio">
    <input type="radio" name="question"> No
  </label>
</div>
```

A radio group = multiple `radio` labels sharing the same `name` attribute (standard HTML behavior). Bulma does not enforce grouping — the `name` does.

---

## `file` (file upload)

A `<div class="file">` containing a `<label class="file-label">` that wraps the native `<input class="file-input" type="file">`. Custom file input, NO JavaScript required (Bulma's file component is intentionally JS-free).

### Anatomy
```html
<div class="file">
  <label class="file-label">
    <input class="file-input" type="file" name="resume">
    <span class="file-cta">
      <span class="file-icon"><i class="fas fa-upload"></i></span>
      <span class="file-label">Choose a file…</span>
    </span>
  </label>
</div>
```

| Class (on the `.file` container) | Effect |
|---|---|
| `file has-name` | Adds a `file-name` span (next to `file-cta`) that shows the selected filename. Render the name statically in markup; to update it on selection, wire a `change` listener yourself. |
| `file is-boxed` | Large drop-area style (icon on top, label below). |
| `file is-small` / `is-medium` / `is-large` | Sizes. |
| `file is-primary`...`is-danger` | Color states. |
| `file is-right` | Filename appears to the right; default is left. |
| `file is-centered` | Centered call-to-action. |

### With a visible filename
```html
<div class="file has-name">
  <label class="file-label">
    <input class="file-input" type="file" name="resume">
    <span class="file-cta">
      <span class="file-icon"><i class="fas fa-upload"></i></span>
      <span class="file-label">Choose a file…</span>
    </span>
    <span class="file-name">resume.pdf</span>
  </label>
</div>
```

The `has-name` variant is the v1 gotcha worth flagging: many users assume Bulma auto-displays the chosen file and reach for JS, but `has-name` + a `file-name` span is the documented, JS-free way to show a filename in the static markup (the dynamic update is your own `change` handler).

---

## Sizes & colors cross-reference

Both apply to the control-type element (`input`, `textarea`, `.select`, `.file`), NOT to `field` or `control`:

- **Sizes:** `is-small`, `is-medium`, `is-large` (default is medium-equivalent).
- **Colors:** `is-primary`, `is-link`, `is-info`, `is-success`, `is-warning`, `is-danger` — the same palette used across Bulma elements.

When a field has icons, the `icon` span's size must match: `icon is-small` for default/small inputs, plain `icon` for `is-medium`, `icon is-large` for `is-large`.

---

## Sources
- https://bulma.io/documentation/form/general/ — `field`, `control`, `is-horizontal`, `is-grouped`, `has-addons`, icons, `is-loading`, colors, sizes, the complete field anatomy.
- https://bulma.io/documentation/form/input/ — `input` class, states, colors, sizes, validation + `help`.
- https://bulma.io/documentation/form/textarea/ — `textarea` class and variants.
- https://bulma.io/documentation/form/select/ — the `.select` wrapper, `is-multiple`, `is-loading` on the wrapper.
- https://bulma.io/documentation/form/checkbox/ — unstyled `checkbox` label.
- https://bulma.io/documentation/form/radio/ — unstyled `radio` label, `checked`.
- https://bulma.io/documentation/form/file/ — `file` anatomy, `has-name`, `is-boxed`, `is-right`, `is-centered`, sizes/colors.
