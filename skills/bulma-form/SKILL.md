---
name: bulma-form
description: Bulma forms. Use when the user says "bulma form" or asks about field, control, input, textarea, select, checkbox, radio, or file upload controls, horizontal forms, field groups, addons, input icons, or field states (is-loading, is-success, is-danger with help text). Includes a complete annotated form example combining fields, states, icons, and addons.
---

# Bulma Forms

## When to use this skill
- The user says "bulma form" or asks how to build, style, or lay out an HTML form with Bulma.
- The user asks about a specific control: `field`, `control`, `input`, `textarea`, `select`, `checkbox`, `radio`, or `file` upload.
- The user asks about form layout: horizontal forms (`is-horizontal`), field grouping (`is-grouped`), or attached controls (`has-addons`).
- The user asks about input icons (`has-icons-left`/`has-icons-right`), loading spinners (`is-loading`), or validation feedback (`is-success`/`is-danger` with `help` text).
- The user wants one end-to-end annotated form combining all of the above (see `references/complete-form-example.md`).
- OUT of scope: overriding Bulma's color tokens or CSS variables that the form states consume → that is theming, owned by `bulma-customize`. Utility spacing/typography helpers applied to form labels (e.g. `mt-4`, `has-text-weight-bold`) → `bulma-helpers`.

## Key classes & concepts
- **The three-layer backbone:** `field` (spacing/grouping wrapper) > `control` (icon/loading/addon host) > the styled native control (`input` / `textarea` / `select` wrapper / `checkbox` / `radio` / `file`). This structure is mandatory — Bulma does NOT auto-style bare `<input>`, `<select>`, or `<textarea>`.
- **Control-type classes:** `input` (on `<input>`), `textarea` (on `<textarea>`), `select` (a `<div class="select">` wrapping a native `<select>`), `checkbox` (on `<label>` around `<input type="checkbox">`), `radio` (on `<label>` around `<input type="radio">`), `file` (a `<div class="file">` wrapping an `<input type="file">`).
- **Field states:** `is-loading` on `control` (spinner); `is-success` / `is-danger` on the control-type element, paired with a sibling `<p class="help is-success|is-danger">` for the message. This is the validation system — do not hand-roll inline-color styles.
- **Sizes:** `is-small`, `is-medium`, `is-large` on the control-type element (`input`, `textarea`, the `.select` wrapper, `.file`).
- **Colors:** `is-primary`, `is-info`, `is-success`, `is-warning`, `is-danger` on the control-type element (focus ring + border recolor).
- **Icons inside controls:** `control has-icons-left` / `has-icons-right`, with a `<span class="icon is-small is-left|is-right"><i class="fas fa-..."></i></span>`. Needs Font Awesome loaded separately.
- **Grouping modifiers on `field`:** `has-addons` (controls attach side by side — e.g. input + submit button), `is-grouped` (controls spaced as a group, with `is-grouped-centered`/`is-grouped-right`), `is-horizontal` (label + field body on one line, using `field-label` + `field-body`).
- **File upload:** `file` + `file-label` + `file-input` + `file-cta` + `file-icon` + `file-label` (the text). `has-name` adds a `file-name` span to show the selected filename without JS. Sizes/colors and `is-boxed` (large drop area) apply.

## Common patterns (copy-paste)

A basic text field (the backbone):

```html
<div class="field">
  <label class="label">Username</label>
  <div class="control">
    <input class="input" type="text" placeholder="e.g. alexsmith">
  </div>
</div>
```

Input with a left icon and a validation state + help text:

```html
<div class="field">
  <label class="label">Email</label>
  <div class="control has-icons-left has-icons-right">
    <input class="input is-danger" type="email" value="alex@">
    <span class="icon is-small is-left"><i class="fas fa-envelope"></i></span>
    <span class="icon is-small is-right"><i class="fas fa-exclamation-triangle"></i></span>
  </div>
  <p class="help is-danger">This email is invalid</p>
</div>
```

Select (note the wrapper `<div class="select">`):

```html
<div class="field">
  <label class="label">Country</label>
  <div class="control">
    <div class="select">
      <select>
        <option>Select dropdown</option>
        <option>With options</option>
      </select>
    </div>
  </div>
</div>
```

Addons (attached input + button) — `has-addons` goes on `field`:

```html
<div class="field has-addons">
  <div class="control">
    <input class="input" type="text" placeholder="Find a repository">
  </div>
  <div class="control">
    <button class="button is-info">Search</button>
  </div>
</div>
```

Loading state — `is-loading` on `control`:

```html
<div class="field">
  <div class="control is-loading">
    <input class="input" type="text" placeholder="Loading...">
  </div>
</div>
```

File upload showing the selected filename (`has-name`):

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

Horizontal field — `is-horizontal` on `field`, with `field-label` + `field-body`:

```html
<div class="field is-horizontal">
  <div class="field-label is-normal">
    <label class="label">From</label>
  </div>
  <div class="field-body">
    <div class="field">
      <div class="control">
        <input class="input" type="text" placeholder="Name">
      </div>
    </div>
  </div>
</div>
```

## v1 gotchas
- **`field` > `control` > control-type is not optional.** Bulma applies no styling to a bare `<input>`, `<select>`, or `<textarea>`. The `field` class is what stacks fields with consistent vertical spacing; the `control` class is what hosts icons, the loading spinner, and addon attachment. Skipping either breaks spacing, icons, and loading — even if the control-type class is present.
- **Validation feedback is color-coupled via `help`.** Put the state on the control-type element (`is-danger` / `is-success`) and the message in a sibling `<p class="help is-danger|is-success">`. Do not inline-style borders or text color — those bypass Bulma's color tokens and break in dark mode.
- **`select` is a wrapper `<div class="select">` around the native `<select>`.** Putting `class="select"` directly on the `<select>` does not work — the wrapper is required for the arrow, sizing, and state styling.
- **`is-loading` goes on `control`, not on `input`.** The spinner is rendered by the control; placing it on the input has no effect.
- **`file has-name` shows the selected filename without JavaScript.** Bulma's file component is intentionally JS-free; wire up `change` events yourself only if you need dynamic filename updates beyond the static markup.
- **`has-addons` vs `is-grouped`:** `has-addons` fuses controls together (no gap, shared border radius); `is-grouped` keeps controls as separate items with spacing and supports `is-grouped-centered`/`is-grouped-right`. Both are modifiers on `field`.
- **Icons require Font Awesome (or another icon font) loaded separately.** Bulma ships the layout (`icon`, `is-left`, `is-right`) but not the glyphs. Also use `is-small`/`is-medium`/`is-large` on the `icon` span to match the input's size.

## References (load on demand)
Load these bundled files when you need full control catalogs, state tables, or the end-to-end capstone. Every file below is part of this skill:
- `references/form-controls.md` — Full catalog of every control (`field`, `control`, `input`, `textarea`, `select`, `checkbox`, `radio`, `file`): classes, states, sizes, colors, grouping modifiers (`has-addons`, `is-grouped`, `is-horizontal`), icons inside controls, and the loading spinner. Use this for any per-control modifier/state lookup.
- `references/complete-form-example.md` — One annotated end-to-end form combining horizontal layout, inputs with icons, validation states with help text, a select, checkboxes and radios, a file upload with `has-name`, addons, and a field button group, with walking commentary on each block. Use this when the user wants a complete, copy-paste form rather than a single control.
