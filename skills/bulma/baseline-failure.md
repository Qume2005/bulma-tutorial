# Baseline-Failure Artifact: `bulma` index/router skill

## Context
This is the RED phase of TDD-for-docs (per `development-team:writing-skills`). It captures a concrete failure that happens when the `bulma` index/router skill does NOT exist — then the skill in `SKILL.md` is the minimal doc that closes it. Document Reviewer enforces this artifact's presence as a PASS/FAIL gate (DoD box 15).

## The failure (what goes wrong without this skill)

The Bulma skill collection ships 7 topic skills (bulma-fundamentals, bulma-layout, bulma-elements, bulma-components, bulma-form, bulma-helpers, bulma-customize). Each topic skill's `description` is narrow and trigger-phrase-specific. Without an index/router that catches GENERAL Bulma requests, the agent exhibits one of two failure modes on broad or ambiguous queries:

### Failure A — agent picks the wrong topic skill (or none)

User query: **"How do I build a Bulma page? Where do I start?"**

With only the 7 narrow topic skills on disk, the agent must match this against 7 narrow descriptions. None of them is a natural fit:
- `bulma-fundamentals` mentions "install" and "class syntax" — but the user didn't say install or syntax.
- `bulma-layout` mentions "arrange content side by side" — but the user didn't say columns.
- The query is TOO BROAD for any single topic skill's trigger phrases.

Observed behavior without a router: the agent either (a) picks an arbitrary topic (e.g. guesses `bulma-fundamentals` and dumps setup info the user didn't ask for), or (b) hallucinates a Bulma answer from model memory, skipping the curated skill collection entirely and risking stale 0.9-era advice (e.g. `@import`-based Sass, direct-value theming — both wrong in v1).

### Failure B — agent gives scattered advice with no routing decision

User query: **"I want a Bulma navbar with a dropdown and dark mode."**

This query spans THREE topic skills: navbar+dropdown → `bulma-components`, dark mode → `bulma-customize`. Without a router that declares the ownership boundaries and a routing rule for multi-topic queries, the agent has no instruction to route first. It may answer directly from memory, mixing component structure (components) with theming (customize) and getting the v1 dark-mode mechanism wrong (e.g. suggesting a plugin, or a JS-based theme switcher, when v1 ships `.theme-dark`/`.theme-light` classes driven by `prefers-color-scheme`).

The failure is: **no single topic skill's description catches "I want a Bulma X with Y and Z", so the agent bypasses the curated collection.**

### Failure C — disambiguation fails on overlapping keywords

User query: **"How do I change the Bulma button color?"**

Three plausible topic skills by keyword:
- `bulma-elements` (owns `button`)
- `bulma-helpers` (owns `has-text-*` color helpers)
- `bulma-customize` (owns overriding `--bulma-*`)

Without explicit disambiguation rules, the agent routes inconsistently: sometimes to helpers (telling the user to add `has-text-primary` to a `<button>`, which is wrong — buttons use `is-primary`), sometimes to customize (telling them to recompile Sass, which is overkill for a single button). The correct answer is `bulma-elements` (button color modifiers `is-primary`/`is-link`/etc.), but only a router with a disambiguation rule ("button → elements, not helpers") gets there reliably.

## Why per-skill descriptions alone don't fix this

The contract (Section 1) makes each topic skill's description trigger-phrase-rich but narrow. That is correct for the topic skills — but it leaves a gap: GENERAL and AMBIGUOUS Bulma requests have no single matching description. The `bulma` index closes that gap with:
1. A BROAD description (Section 1.1) that catches any Bulma mention and promises routing.
2. A routing table (Section 4.1) mapping intents → sibling skill → why.
3. Disambiguation rules (Section 4.2) for the overlapping-keyword cases (button→elements, color helpers vs theming, responsive grid→layout, dark mode→customize, `.block`→elements, form states→form).

## The minimal skill that closes it

`SKILL.md` (this directory) is the minimal router:
- Frontmatter: `name: bulma` + the broad Section 1.1 description (catches general Bulma requests, says it routes to bulma-* skills).
- Body: the Section 4.1 routing table (all 7 siblings), the Section 4.2 disambiguation rules (ordered), and the one-line routing reminder.
- NO `references/` directory, NO class catalog, NO v1-gotchas section — those belong to the topic skills. The index only routes.

With this skill present: Failure A → router catches "build a Bulma page" and routes to fundamentals as the default entry point (disambiguation rule 8). Failure B → router catches the multi-topic query, and the routing reminder forces a route-first decision (the user gets the components route for navbar/dropdown and is pointed to customize for dark mode). Failure C → disambiguation rules 2, 5, 6, 7 resolve the button-color case to `bulma-elements`.

## What this artifact is NOT
- It is not a claim that the topic skills are badly written — they are correctly narrow. The gap is the index/router layer that no topic skill owns.
- It is not a general "routing is good" essay — it names three specific query types that fail TODAY without the router and the exact wrong behavior each produces.

## References
- Contract: `/Users/liqianmo/projects/bulma-tutorial/.claude/development-team/planner/bulma-skill-collection-contract-june-16th-2026.md` (Section 1.1 frontmatter, Section 4 routing table + disambiguation, Section 7 DoD boxes 14–16).
- Authoring discipline: `development-team:writing-skills` (RED phase: concrete baseline-failure artifact before the minimal doc).
- Pattern precedent: postman-routing (broad-description router + routing-table body; no deep content in the index).
