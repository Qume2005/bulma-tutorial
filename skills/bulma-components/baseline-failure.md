# Baseline-Failure Artifact — `bulma-components`

## Context

Per `development-team:writing-skills` (TDD-for-docs), this is the RED artifact: a concrete, real-world failure the `bulma-components` skill must prevent. The skill is the minimal doc that closes this failure. Written BEFORE the skill.

## The failure

A developer reads the Bulma docs and builds a navbar with a burger menu and a dropdown. They copy the HTML structure exactly:

```html
<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="/">Brand</a>
    <a role="button" class="navbar-burger" aria-label="menu" aria-expanded="false" data-target="navbarMenu">
      <span></span><span></span><span></span>
    </a>
  </div>
  <div id="navbarMenu" class="navbar-menu">
    <div class="navbar-start">
      <!-- dropdown -->
      <div class="navbar-item has-dropdown is-hoverable">
        <a class="navbar-link">More</a>
        <div class="navbar-dropdown">
          <a class="navbar-item">About</a>
          <a class="navbar-item">Jobs</a>
        </div>
      </div>
    </div>
  </div>
</nav>
```

**Observed behavior (broken):**

1. On mobile, tapping the burger (hamburger) does NOTHING. The `navbar-menu` never appears. The three lines never become an X.
2. They then add a standalone `dropdown` component elsewhere and find clicking the trigger does nothing — the menu never opens.

**Root cause:** Bulma is a **CSS-only framework — it ships zero JavaScript.** The `is-active` class that drives the burger→menu toggle, the dropdown open/close, and the modal show/hide must be toggled by JavaScript the developer writes. The HTML structure is necessary but NOT sufficient. Without the toggle script:

- `.navbar-burger` stays an inert hamburger (CSS only animates the lines when `is-active` is present).
- `.navbar-menu` stays `display: none` on mobile (it only shows on desktop, OR when `.is-active` is added).
- `.dropdown` never shows `.dropdown-menu` without `.is-active` on the `.dropdown` container.
- `.modal` is invisible until `.is-active` is added to the `.modal` container.

This is the single most common "Bulma is broken" report from new users — they assume Bulma is like Bootstrap (which bundles `bootstrap.js`) and that the components "just work." They do not.

## The correction this skill must deliver

The skill must make three things unavoidable for the reader:

1. **State the rule up front:** Bulma ships NO JavaScript. `is-active` is the universal toggle class. Every interactive component (navbar burger, dropdown, modal) needs a few lines of vanilla JS the developer adds.
2. **Provide the minimal working JS snippets** (copy-paste) for:
   - Navbar burger → toggle `is-active` on both `.navbar-burger` and the target `.navbar-menu`.
   - Dropdown → toggle `is-active` on the `.dropdown` container.
   - Modal → add/remove `is-active` on the `.modal` container (open on trigger click, close on background/close-button click).
3. **Document the full HTML structure** for all 10 components so the structure is correct before the JS layer is even considered. Structural correctness (which class nests where) is the other half of "it just works."

If the skill does any one of these three poorly, the developer is back to the broken state above.

## What is OUT of scope for this skill (owned by siblings)

- Deep theming of component colors via `--bulma-*` variables → `bulma-customize`.
- The `is-active` *modifier concept* (states/modifiers generally) → `bulma-fundamentals`.
- Form controls placed inside navbars/panels → `bulma-form`.
- Layout wrappers (`hero`, `section`, `container`) around components → `bulma-layout`.

## Why this artifact is specific

It names the exact classes (`navbar-burger`, `navbar-menu`, `dropdown`, `modal`, `is-active`), quotes the exact symptom ("tapping the burger does nothing"), and identifies the exact root cause ("Bulma ships zero JavaScript"). It is not a generic "components are hard" complaint.

## References

- Bulma navbar (official JS snippet lives here): https://bulma.io/documentation/components/navbar/
- Bulma dropdown: https://bulma.io/documentation/components/dropdown/
- Bulma modal: https://bulma.io/documentation/components/modal/
- Authoring discipline: `development-team:writing-skills`
