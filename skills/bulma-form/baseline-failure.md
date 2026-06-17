# Baseline-Failure Artifact: `bulma-form`

## Context
This is the TDD-for-docs RED artifact (per `development-team:writing-skills`) for the `bulma-form` skill. It documents a concrete, real-world wrong Bulma form pattern that the skill must prevent — shown BEFORE the skill's correction. The skill is the minimal doc that closes this failure.

## The Failure (what a user actually does)

A user who has skimmed a Bulma tutorial writes a form by dropping raw native HTML controls into a `<form>` and expects Bulma to style them, with no `field`/`control` wrappers, no `input` class on the `<input>`, and ad-hoc `style="margin"` hacks for spacing. They also try to show a validation error with an inline `<span style="color:red">`.

```html
<!-- BROKEN: no field/control wrappers, no input/select classes, no help text -->
<form>
  <label>Username</label>
  <input type="text" placeholder="e.g. alexsmith" style="margin-bottom: 1.5rem">
  <label>Email</label>
  <input type="email" placeholder="e.g. alexsmith@gmail.com" style="margin-bottom: 1.5rem; border-color: red">
  <span style="color: red; display: block; margin-bottom: 1.5rem;">This email is invalid</span>
  <select>
    <option>Country</option>
    <option>United States</option>
  </select>
  <button>Submit</button>
</form>
```

## Why it fails (concretely)

1. **No styling applied.** Bulma does NOT auto-style raw `<input>`, `<select>`, or `<textarea>`. The user MUST add the matching Bulma class (`input`, `select`, `textarea`). Without `class="input"`, the input renders as plain browser default — no Bulma padding, border, focus ring, or radius.
2. **No vertical rhythm.** Bulma's spacing model for forms is `field` (vertical stacker — adds `margin-bottom` between fields) wrapping `control` (which owns icons, loading spinners, and addons). Raw inputs with `style="margin-bottom"` bypasses this entirely, producing inconsistent gaps, broken icon alignment, and no loading spinner / addon support.
3. **No icon / loading / addon support.** `control has-icons-left`, `control is-loading`, and `field has-addons` ONLY work inside the `field > control > input` structure. The user's raw layout can never host an icon or a spinner or an addon.
4. **Validation state is hand-rolled.** Bulma's documented validation pattern is `input is-danger` + `<p class="help is-danger">message</p>` — a single, consistent, color-coupled mechanism. The user's `style="border-color:red"` + `<span style="color:red">` duplicates effort, drifts from Bulma's color tokens, breaks in dark mode, and is invisible to the `is-loading`/`is-success`/`is-danger` system.
5. **Select and button are unstyled.** `select` needs `class="select"` (the wrapper `<div class="select">`), and the button needs `class="button"`. Neither renders as Bulma without it.

## What the correct pattern looks like (the GREEN the skill teaches)

```html
<form>
  <div class="field">
    <label class="label">Username</label>
    <div class="control has-icons-left">
      <input class="input" type="text" placeholder="e.g. alexsmith">
      <span class="icon is-small is-left"><i class="fas fa-user"></i></span>
    </div>
  </div>

  <div class="field">
    <label class="label">Email</label>
    <div class="control has-icons-left has-icons-right">
      <input class="input is-danger" type="email" placeholder="Email input" value="alex@">
      <span class="icon is-small is-left"><i class="fas fa-envelope"></i></span>
      <span class="icon is-small is-right"><i class="fas fa-exclamation-triangle"></i></span>
    </div>
    <p class="help is-danger">This email is invalid</p>
  </div>

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

  <div class="field">
    <div class="control">
      <button class="button is-primary">Submit</button>
    </div>
  </div>
</form>
```

## The delta this skill must deliver

- The **`field` > `control` > `input`** three-layer structure as the mandatory backbone (field = spacing/grouping, control = icon/loading/addon host, input/select/textarea = the styled native element).
- The matching class rule: every styled native control needs its Bulma class (`input`, `textarea`, `select` wrapper, `checkbox`, `radio`, `file`).
- The validation/state system: `is-loading` on control, `is-success`/`is-danger` on the input paired with `help is-success`/`help is-danger` text — NEVER inline styles for this.
- Grouping modifiers: `is-grouped`, `has-addons`, `is-horizontal` — each a `field` modifier, not a control property.
- `file has-name` for showing the selected filename without JS.
- A complete annotated capstone form that combines all of the above (→ `references/complete-form-example.md`).

## Sourcing
All patterns above are verified against bulma.io:
- https://bulma.io/documentation/form/general/ (field/control structure, is-horizontal, is-grouped, has-addons, icons, is-loading, colors, sizes)
- https://bulma.io/documentation/form/input/ (input class, is-danger/is-success + help)
- https://bulma.io/documentation/form/textarea/
- https://bulma.io/documentation/form/select/ (the `<div class="select">` wrapper)
- https://bulma.io/documentation/form/checkbox/
- https://bulma.io/documentation/form/radio/
- https://bulma.io/documentation/form/file/ (has-name variant)
