# Complete Form Example (Capstone)

> Reference file for the `bulma-form` skill. One annotated end-to-end form that combines horizontal layout, inputs with icons, validation states with `help` text, a select, checkboxes and radios, a file upload with `has-name`, addons, and a field button group. Walking commentary on each block. Sourced from bulma.io.

This is the complete form. Each numbered block below is explained immediately after the snippet. Load the font icon library (Font Awesome) separately — Bulma ships the layout, not the glyphs.

## The full form

```html
<form>
  <!-- 1. Horizontal field: From (text, left icon) + To (text, left icon) in one field-body -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">From</label>
    </div>
    <div class="field-body">
      <div class="field is-expanded">
        <div class="control has-icons-left">
          <input class="input" type="text" placeholder="Name">
          <span class="icon is-small is-left"><i class="fas fa-user"></i></span>
        </div>
      </div>
    </div>
  </div>

  <!-- 2. Email with left + right icons and a danger validation state + help text -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Email</label>
    </div>
    <div class="field-body">
      <div class="field is-expanded">
        <div class="control has-icons-left has-icons-right">
          <input class="input is-danger" type="email" placeholder="Email" value="alex@">
          <span class="icon is-small is-left"><i class="fas fa-envelope"></i></span>
          <span class="icon is-small is-right"><i class="fas fa-exclamation-triangle"></i></span>
        </div>
        <p class="help is-danger">This email is invalid</p>
      </div>
    </div>
  </div>

  <!-- 3. Select dropdown (note the .select wrapper) -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Country</label>
    </div>
    <div class="field-body">
      <div class="field">
        <div class="control">
          <div class="select">
            <select>
              <option>Select dropdown</option>
              <option>With options</option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 4. Textarea -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Message</label>
    </div>
    <div class="field-body">
      <div class="field">
        <div class="control">
          <textarea class="textarea" placeholder="Tell us what you are thinking"></textarea>
        </div>
      </div>
    </div>
  </div>

  <!-- 5. Addons: URL input + submit button, fused -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Website</label>
    </div>
    <div class="field-body">
      <div class="field has-addons">
        <div class="control">
          <span class="button is-static">https://</span>
        </div>
        <div class="control is-expanded">
          <input class="input" type="text" placeholder="yoursite.com">
        </div>
      </div>
    </div>
  </div>

  <!-- 6. File upload with has-name -->
  <div class="field is-horizontal">
    <div class="field-label is-normal">
      <label class="label">Resume</label>
    </div>
    <div class="field-body">
      <div class="field">
        <div class="control">
          <div class="file has-name is-fullwidth">
            <label class="file-label">
              <input class="file-input" type="file" name="resume">
              <span class="file-cta">
                <span class="file-icon"><i class="fas fa-upload"></i></span>
                <span class="file-label">Choose a file…</span>
              </span>
              <span class="file-name">resume.pdf</span>
            </label>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- 7. Checkboxes -->
  <div class="field is-horizontal">
    <div class="field-label">
      <label class="label">Subscribe</label>
    </div>
    <div class="field-body">
      <div class="field">
        <div class="control">
          <label class="checkbox">
            <input type="checkbox"> Email newsletters
          </label>
          <label class="checkbox">
            <input type="checkbox"> Product updates
          </label>
        </div>
      </div>
    </div>
  </div>

  <!-- 8. Radios (grouped by shared name) -->
  <div class="field is-horizontal">
    <div class="field-label">
      <label class="label">Plan</label>
    </div>
    <div class="field-body">
      <div class="field">
        <div class="control">
          <label class="radio">
            <input type="radio" name="plan" checked> Free
          </label>
          <label class="radio">
            <input type="radio" name="plan"> Pro
          </label>
          <label class="radio">
            <input type="radio" name="plan"> Enterprise
          </label>
        </div>
      </div>
    </div>
  </div>

  <!-- 9. Field button group (is-grouped, right-aligned) -->
  <div class="field is-horizontal">
    <div class="field-label"></div>
    <div class="field-body">
      <div class="field is-grouped is-grouped-right">
        <div class="control">
          <button class="button is-light">Cancel</button>
        </div>
        <div class="control">
          <button class="button is-primary">Submit</button>
        </div>
      </div>
    </div>
  </div>
</form>
```

## Block-by-block commentary

**1. Horizontal text field with a left icon.** `field is-horizontal` puts the label and body on one line. `field-label is-normal` sizes the label column. Inside `field-body`, a `field is-expanded` + `control is-expanded` lets the input fill the available width. The `has-icons-left` + `<span class="icon is-small is-left">` adds the user glyph. Note `is-expanded` — without it the field would shrink to its content.

**2. Email with left + right icons and a validation state.** This is the canonical Bulma validation pattern. The `input is-danger` recolors the border; the right icon (`fa-exclamation-triangle`) is the visual warning. The `p help is-danger` is the message, color-coupled to the input border so they never drift apart. `is-success` works identically (pair with `help is-success` and a check glyph). Do NOT inline-style the border or text color.

**3. Select dropdown.** Two nested wrappers: `control` > `<div class="select">` > native `<select>`. The `.select` wrapper is what Bulma styles (arrow glyph, sizing, focus ring). Putting `class="select"` directly on the `<select>` will NOT work.

**4. Textarea.** `textarea` class on a native `<textarea>`. Same `field` > `control` structure. Sizing via native `rows`.

**5. Addons (fused URL + submit).** `field has-addons` fuses the children. A `button is-static` serves as a non-interactive prefix (the `https://`). `control is-expanded` on the input makes it absorb the remaining width. Contrast with `is-grouped` (block 9), which keeps controls separate with gaps — addons are for controls that form ONE logical input.

**6. File upload with `has-name`.** `file has-name` reserves space for a `file-name` span. The `file-cta` (call to action) holds the icon and the button-like label. `is-fullwidth` stretches the component. The filename shown in `file-name` is static markup; to update it on selection, attach a `change` listener that sets the `file-name` text — Bulma provides the layout, not the JS.

**7. Checkboxes.** Each `label.checkbox` wraps an `<input type="checkbox">`. Multiple checkboxes live in one `control`; they stack via the field spacing. Bulma intentionally does not style the checkbox glyph itself (cross-browser fidelity), only the label.

**8. Radios.** Each `label.radio` wraps an `<input type="radio">`. Grouping is done by the shared `name="plan"` attribute (standard HTML) — one may be `checked` by default. Like checkboxes, the glyph is left native.

**9. Button group (`is-grouped`, right-aligned).** `field is-grouped is-grouped-right` keeps the Cancel and Submit buttons as separate items with spacing, pushed to the right edge. Use `is-grouped` (not `has-addons`) for distinct actions. An empty `field-label` keeps the column aligned with the rows above.

## Putting it together — the patterns this example exercises

| Pattern | Block | Key classes |
|---|---|---|
| Horizontal layout | 1–9 | `field is-horizontal`, `field-label`, `field-body` |
| Left icon | 1 | `control has-icons-left`, `icon is-small is-left` |
| Left + right icons + validation | 2 | `has-icons-left has-icons-right`, `input is-danger`, `help is-danger` |
| Select (wrapper rule) | 3 | `control` > `div.select` > `select` |
| Textarea | 4 | `textarea` |
| Addons (fused) | 5 | `field has-addons`, `control is-expanded`, `button is-static` |
| File with filename | 6 | `file has-name`, `file-cta`, `file-name` |
| Checkboxes | 7 | `label.checkbox` |
| Radios (shared name) | 8 | `label.radio`, `name=...`, `checked` |
| Button group (spaced) | 9 | `field is-grouped is-grouped-right` |

## Sources
- https://bulma.io/documentation/form/general/ — the horizontal/addon/grouped/icon/loading/state patterns.
- https://bulma.io/documentation/form/input/ — validation (`is-danger`/`is-success`) + `help` pairing.
- https://bulma.io/documentation/form/select/ — the `.select` wrapper rule.
- https://bulma.io/documentation/form/textarea/ — `textarea`.
- https://bulma.io/documentation/form/checkbox/ — `checkbox`.
- https://bulma.io/documentation/form/radio/ — `radio` + `checked`.
- https://bulma.io/documentation/form/file/ — `has-name`, `file-cta`, `file-name`, `is-fullwidth`.
