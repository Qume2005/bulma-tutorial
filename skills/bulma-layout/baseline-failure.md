# Baseline-Failure Artifact: bulma-layout

## Context

Per `development-team:writing-skills` (TDD-for-docs), this file is written FIRST. It captures a concrete, real wrong/stale Bulma pattern that the `bulma-layout` skill must prevent. The skill is then written as the MINIMAL doc that closes this failure. The Document Reviewer enforces this artifact's presence as a PASS/FAIL gate.

The `bulma-layout` skill covers two systems that are easy to conflate, and one of them (CSS Grid) is NEW in Bulma 1.0 and absent from most older tutorials. The baseline failure targets the most damaging real-world mistake: assuming Bulma "has no grid" / forcing a 12-column CSS Grid layout through the Flexbox `columns` system, which was never designed for it.

---

## The Failure (concrete, shown BEFORE the correction)

### Scenario

A developer is building a responsive dashboard with exactly 12 fixed columns. Each tile must snap to predictable, equal-width cells that reflow cleanly by breakpoint (e.g. 1 cell = 1 column on mobile, up to 12 columns on desktop). The developer has only ever seen the Flexbox `columns` API in older (0.9-era) tutorials.

### What they write (the wrong path)

```html
<!-- WRONG: forcing a fixed 12-column CSS Grid through the Flexbox columns system -->
<div class="columns is-multiline is-gapless">
  <div class="column is-1">Tile 1</div>   <!-- "is-1" = 1/12 width, flexbox-driven -->
  <div class="column is-2">Tile 2</div>
  <div class="column is-1">Tile 3</div>
  <div class="column is-8">Tile 4</div>
  <div class="column is-1">Tile 5</div>
  <div class="column is-1">Tile 6</div>
  <div class="column is-1">Tile 7</div>
  <!-- ... -->
</div>
```

### Why it fails / degrades

1. **The mental model is wrong.** Bulma's `columns` is a **Flexbox** one-dimensional system. Its `is-N` sizes are fractional widths (`is-1` = 1 of 12 fractions), computed via flex-grow — not a real CSS Grid track. The developer believes they have a grid; they have a flex row.
2. **Row wrapping is manual and brittle.** Because `columns` is 1D, rows only "break" via `is-multiline` heuristics and explicit fractional sums. A 12-tile dense layout (where some tiles span 2 cols, some 1, with full-height rows) cannot express row/column spanning. There is no `is-col-span-*` / `is-row-span-*` in the Flexbox columns API.
3. **Vertical alignment is a constant fight.** Flex columns don't give you 2D placement, so tiles that should occupy specific grid intersections require nested `columns` rows + hacks, producing fragile, non-reflowing markup.
4. **The user concludes "Bulma has no grid."** This is the baseline failure the skill exists to correct: Bulma 1.0 **does** ship a real CSS Grid system (`grid` / `cell` / `smart-grid` / `fixed-grid`) that does exactly what they need — but because it is NEW in v1 and absent from 0.9-era material (and from the STALE context7 0.9.3 snapshot), a model trained on older content will never surface it.

### The stale-knowledge angle

A model with no `bulma-layout` skill, drawing on pre-v1 knowledge, will:
- answer "how do I make a 12-column grid in Bulma" with the Flexbox `columns` recipe above, and
- never mention `fixed-grid` + `has-auto-count` (which auto-reflows to 2/4/8/12/16 columns by breakpoint) or the Smart Grid (a bare `grid` with `is-col-min-*` that auto-fits columns to a minimum width), and
- conflate the two systems when the user says "bulma grid."

This is exactly the conflation the Section 5 v1-accuracy rule (#4) forbids: *"the v1 CSS Grid system exists ALONGSIDE the Flexbox columns system. They are distinct; do not conflate."*

---

## What the correction looks like (the skill's job — NOT spelled out here)

The `bulma-layout` skill must, at minimum:

1. Name BOTH systems up front and state they are distinct (Flexbox `columns` vs CSS Grid `grid`/`cell`).
2. Give the ONE-snip decision rule: use `fixed-grid`/`smart-grid` for fixed/equal-cell 2D layouts with spanning; use `columns` for content-driven, variable-width flex rows.
3. Show a minimal `fixed-grid` + `cell` example that closes the dashboard scenario (with `has-auto-count` for breakpoint reflow).
4. Point to `references/grid.md` for the full `is-col-span-*` / `is-row-span-*` cell catalog, and `references/columns.md` for the Flexbox size/offset/multiline catalog.

The reference catalogs and the SKILL.md body that delivers items 1-4 are GREEN. This baseline-failure artifact is RED.

---

## v1 facts this failure depends on (all verifiable on bulma.io)

- The CSS Grid system is NEW in Bulma 1.0 — lives at `https://bulma.io/documentation/grid/` (overview), with `/grid/grid-cells/`, `/grid/smart-grid/`, `/grid/fixed-grid/`.
- `fixed-grid` supports a `has-auto-count` modifier that sets the column count per breakpoint (2 mobile / 4 tablet / 8 desktop / 12 widescreen / 16 fullhd), or pin a count yourself with `has-N-cols` (1–12).
- The Smart Grid is the default `grid` (a bare `<div class="grid">` auto-fits columns to a minimum width set by `is-col-min-*`); there is no separate `smart-grid` wrapper class.
- Grid cells support `is-col-span-*` / `is-row-span-*` per breakpoint.
- The Flexbox `columns` system predates Grid and is unchanged in purpose (1D flexible row) — `https://bulma.io/documentation/columns/`.

(Note: the contract's Section 6 guessed the Grid URL as `features/grid-system/` — that is wrong. The verified path is `/documentation/grid/`. Flagged in the return summary.)

---

## References

- Contract: `/Users/liqianmo/projects/bulma-tutorial/.claude/development-team/planner/bulma-skill-collection-contract-june-16th-2026.md` (Writer 3 spec, Section 5 v1 fact #4)
- Bulma Grid overview: https://bulma.io/documentation/grid/
- Bulma Grid Cells: https://bulma.io/documentation/grid/grid-cells/
- Bulma Smart Grid: https://bulma.io/documentation/grid/smart-grid/
- Bulma Fixed Grid: https://bulma.io/documentation/grid/fixed-grid/
- Bulma Columns (Flexbox): https://bulma.io/documentation/columns/
