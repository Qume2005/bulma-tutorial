---
name: bulma
description: Bulma CSS (v1.x) tutorial index and router. Use when the user mentions Bulma or asks how to build, style, or customize a Bulma page, navbar, hero, card, button, table, form, modal, columns, grid, responsive layout, or how to install Bulma, override Bulma colors, set up dark mode, or migrate Bulma from 0.9 to 1.0. Routes the request to the matching bulma-* skill (bulma-fundamentals, bulma-layout, bulma-elements, bulma-components, bulma-form, bulma-helpers, bulma-customize). Do not answer Bulma questions directly from this skill — route first.
---

# Bulma Tutorial Index & Router

> This skill is a **router**. Always route to a `bulma-*` topic skill using the table below; do not answer Bulma questions directly from this skill. The topic skills carry the actual classes, examples, and v1 details.

This is the entry point for the Bulma (v1.0.4) skill collection. Bulma is a free, open-source CSS framework based on Flexbox (and, as of v1.0, also CSS Grid). When a user asks anything about Bulma, this skill decides which of the 7 topic skills owns the answer. Match the user's intent against the routing table; if two skills look plausible, apply the disambiguation rules in order.

## When to use this skill

- The user mentions "Bulma" at all — install, build, style, customize, migrate, or any Bulma class/element/component.
- The user asks a broad Bulma question that doesn't obviously fit one topic ("how do I build a Bulma page?", "where do I start with Bulma?").
- The user names something that could belong to more than one topic (e.g. "button color", "responsive grid", "dark mode") — apply the disambiguation rules.

This skill does NOT answer Bulma questions itself. It only routes. If you find yourself about to explain a Bulma class, write an HTML snippet, or give v1 theming advice from this skill, stop — you should have routed to a topic skill.

## Routing table

Match the user's phrasing/intent to a row. The "Why" column explains the ownership boundary.

| User intent / phrasing | Route to | Why |
|---|---|---|
| "install Bulma", "bulma setup", "how Bulma classes work", "bulma modifiers", "is-/has- modifier ecosystem", "bulma colors" (the 8 theme colors as a concept), "bulma breakpoints", "responsive in Bulma" (concept), "how do I build a Bulma page" (default entry) | `bulma-fundamentals` | Setup, class syntax, modifier ecosystem, the 8 built-in theme colors, breakpoints — the foundation. |
| "bulma columns", "12-column", "responsive columns", "bulma grid", "cell", "smart-grid", "fixed-grid", "container", "level", "media object", "hero", "section", "footer", "arrange side by side", "page structure" | `bulma-layout` | Flexbox `columns` system + the v1 CSS Grid system (`grid`/`cell`/`smart-grid`/`fixed-grid`) + page-structure elements. |
| "bulma button", "box", "block" (the element), "content", "delete icon", "icon", "image", "notification", "progress bar", "table", "tag", "title", "subtitle" | `bulma-elements` | The 12 standalone elements (button, box, block, content, delete, icon, image, notification, progress, table, tag, title/subtitle). |
| "bulma navbar", "breadcrumb", "card", "dropdown", "menu", "message", "modal", "pagination", "panel", "tabs" | `bulma-components` | The 10 composite components — most need a few lines of JavaScript to toggle `is-active`. |
| "bulma form", "field", "control", "input", "textarea", "select", "checkbox", "radio", "file upload", "form validation states", "horizontal form", "addons", "is-grouped" | `bulma-form` | All form controls + the annotated end-to-end form example. |
| "has-text-*", "has-background-*", "bulma color helpers" (the utility classes), "typography helpers", "spacing helpers", "margin/padding helpers" (`m*`/`p*`), "flex helpers", "is-hidden-*", "skeleton", "utility classes" | `bulma-helpers` | Tiny utility classes applicable to any element; also owns skeleton loaders. |
| "customize Bulma", "override Bulma colors", "bulma sass variables", "@use bulma", "dart sass", "bulma dark mode", "bulma themes", "color palettes" (the concept), "--bulma-*", "migrate Bulma 0.9 to 1.0", "bulma migration" | `bulma-customize` | Advanced theming via `--bulma-*` CSS variables, Sass build config (Dart Sass), dark mode, themes, and the 0.9→1.0 migration. |

## Disambiguation rules (apply in order)

When a query matches two or more rows, resolve using these rules top-down:

1. **Two plausible skills → pick the MORE SPECIFIC topic skill.** The index only routes; it never answers. If the question is clearly about one topic, route to that topic skill even if a keyword also matches this index's broad description.

2. **Color helpers vs color theming.** `has-text-*` / `has-background-*` / palette-* helper CLASSES → `bulma-helpers`. Defining new colors, overriding `--bulma-*`, custom palettes (the concept), Sass color variables → `bulma-customize`. Fundamentals owns the 8 built-in theme colors at a *concept* level only (names + hex + usage), not the override recipe.

3. **CSS variables intro vs deep theming.** Fundamentals *introduces* `--bulma-*` briefly. Anything actionable about overriding them, dark mode, or themes → `bulma-customize`. Do not duplicate the override recipe in fundamentals.

4. **Skeletons** → `bulma-helpers` (owns skeletons exclusively).

5. **Form field states** (`is-success` / `is-danger` / `is-loading` with `help` text) → `bulma-form`, even though they look like modifier usage (fundamentals) or color helpers. Form owns the field-state + help-text pattern.

6. **`.block` element** → `bulma-elements` (it is an element, new in v1, the standard vertical-rhythm spacer). Do not confuse with spacing helpers (`m*`/`p*` in `bulma-helpers`) — `.block` is a structural element, `m*`/`p*` are utility classes.

7. **Navbar / dropdown / modal / tabs interactivity** → `bulma-components` owns the JS snippets, even if the user frames it as "JavaScript" or "toggle". Bulma ships no JS framework; the few lines of toggle code live in the components skill.

8. **Broad "how do I build a Bulma page"** → this index catches it; route to `bulma-fundamentals` as the default entry point, then let the user's specifics pull in layout/elements/components.

9. **Migration** → always `bulma-customize`, never fundamentals. Fundamentals only carries the brief "migrating-to-v" pointer at the concept level; the actual migration checklist (Sass `@import`→`@use`, CSS variables, LibSass dropped, Grid/`.block` additions) lives in customize.

## Common ambiguous cases (quick reference)

| User says | Route to | Not to |
|---|---|---|
| "bulma button color" / "is-primary button" | `bulma-elements` | not helpers (buttons use `is-*`, not `has-text-*`) |
| "responsive grid" / "bulma grid" | `bulma-layout` | not fundamentals (breakpoints ≠ grid) |
| "dark mode" / "customize colors" / "override theme" | `bulma-customize` | not fundamentals (only introduces the concept) |
| "spacing between sections" / ".block" | `bulma-elements` | not helpers (`.block` is an element, not `m*`/`p*`) |
| "margin-top on a div" / "mx-4" | `bulma-helpers` | not elements (spacing utilities, not `.block`) |
| "navbar burger menu" / "toggle dropdown" | `bulma-components` | not fundamentals (the JS lives here) |
| "form validation red/green input" | `bulma-form` | not helpers or fundamentals (field-state + `help` pattern) |
| "skeleton loader" | `bulma-helpers` | not elements (skeletons owned here exclusively) |
| "migrate from 0.9" / "@import to @use" | `bulma-customize` | not fundamentals |
| "is-primary / is-large modifier generally" | `bulma-fundamentals` | — (modifier ecosystem lives here) |

## What this index does NOT contain

- No Bulma class catalogs, no HTML snippets, no v1 theming recipes. Those live in the topic skills.
- No `references/` directory. This is a single-file router.
- If you need the actual classes or examples, you have routed to the wrong skill — re-route.

## Sibling skills (the 7 topic skills this router dispatches to)

- `bulma-fundamentals` — install, class syntax, modifier ecosystem, 8 theme colors, breakpoints.
- `bulma-layout` — Flexbox columns, v1 CSS Grid, container/level/media-object/hero/section/footer.
- `bulma-elements` — the 12 elements (block, box, button, content, delete, icon, image, notification, progress, table, tag, title/subtitle).
- `bulma-components` — the 10 components (breadcrumb, card, dropdown, menu, message, modal, navbar, pagination, panel, tabs), incl. the small JS for interactivity.
- `bulma-form` — field/control/input/textarea/select/checkbox/radio/file, states, addons, horizontal forms, the annotated form.
- `bulma-helpers` — color/typography/spacing/flex/visibility utility classes + skeletons.
- `bulma-customize` — `--bulma-*` CSS variables, Sass (Dart Sass, `@use`/`@forward`), dark mode, themes, color palettes, 0.9→1.0 migration.
