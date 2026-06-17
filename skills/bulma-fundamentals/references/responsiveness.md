# Reference: Bulma Responsiveness & Breakpoints

> Bundled with `bulma-fundamentals`. Covers the 5 screen sizes, their exact pixel values, the `is-*-only` variants, the `is-widescreen`/`is-fullhd` upward modifiers, and the `container` max-width behavior per breakpoint.

## 1. The 5 breakpoints

Bulma is **mobile-first**: base styles target mobile, and larger breakpoints add/override via min-width media queries. There are **4 breakpoints** defining **5 screen sizes**:

| Screen size | Range | Applies (min-width) |
|---|---|---|
| **mobile** | up to 768px | (default, no media query) |
| **tablet** | 769px – 1023px | `≥ 769px` |
| **desktop** | 1024px – 1215px | `≥ 1024px` |
| **widescreen** | 1216px – 1407px | `≥ 1216px` |
| **fullhd** | 1408px and up | `≥ 1408px` |

Two of the larger breakpoints are **toggleable** at the Sass level (owned by `bulma-customize`):

```scss
$widescreen-enabled: false; // disables widescreen + fullhd cascade parts
$fullhd-enabled: false;     // disables fullhd
```

## 2. Upward variants (`is-*` by breakpoint)

Because Bulma is mobile-first, a breakpoint suffix means **"from this size and up"**:

| Suffix | Means "from … up" |
|---|---|
| `is-mobile` | mobile and up (effectively always — present for explicitness) |
| `is-tablet` | ≥ 769px |
| `is-desktop` | ≥ 1024px |
| `is-widescreen` | ≥ 1216px |
| `is-fullhd` | ≥ 1408px |

Used on responsive classes (mostly helpers — `bulma-helpers` owns the full list):

```html
<p class="is-size-3-mobile is-size-5-desktop">
  Bigger on mobile, smaller from desktop up.
</p>
<div class="is-hidden-widescreen">Hidden only once you reach widescreen? No — hidden from widescreen UP.</div>
```

> Note: `is-hidden-widescreen` means hidden at widescreen AND larger. To hide in a single band, use the `*-only` variant below.

## 3. `*-only` variants (a single breakpoint band)

To target exactly one screen size (not "and up"), append `-only`:

| Variant | Active only in band |
|---|---|
| `is-mobile-only` | ≤ 768px |
| `is-tablet-only` | 769–1023px |
| `is-desktop-only` | 1024–1215px |
| `is-widescreen-only` | 1216–1407px |

(`is-fullhd` is already the top band, so there is no `is-fullhd-only` — it means ≥ 1408px.)

Common use — show a burger menu only on mobile, hide it from tablet up:

```html
<button class="is-hidden-tablet">☰</button>
```

## 4. The `container` element and max-widths

`container` is Bulma's centered content wrapper. Its behavior changes per breakpoint (it has horizontal `margin: auto` and a max-width that kicks in at each larger size):

| Breakpoint | `.container` max-width |
|---|---|
| mobile (≤ 768px) | full width (fluid) |
| tablet (≥ 769px) | up to 704px content |
| desktop (≥ 1024px) | up to 944px content |
| widescreen (≥ 1216px) | up to 1128px content |
| fullhd (≥ 1408px) | up to 1344px content |

Two modifier variants:

- `container is-widescreen` — the container becomes full-width up to the widescreen breakpoint (drops the desktop max-width cap).
- `container is-max-desktop` — caps the container at the desktop max-width even on larger screens (useful for readable article widths).
- `container is-max-widescreen` — caps at the widescreen max-width even on fullhd.

(The layout mechanics of `container`, `columns`, `grid`, `hero`, `section`, `footer` are owned by `bulma-layout`. This table is the responsiveness behavior only.)

## 5. v1 notes
- The breakpoint pixel values are **unchanged from 0.9**; what's new in v1 is that `widescreen`/`fullhd`-enabled flags and the CSS-variable-driven sizes exist, but the min-width thresholds above are stable.
- Responsive variants on helpers (`is-hidden-*-mobile`, `is-size-*-desktop`, spacing `mx-*-tablet`, flex direction per breakpoint) are cataloged in `bulma-helpers`, not here.
- Responsive **columns** behavior (`is-three-quarters-mobile`, column gaps per breakpoint) is in `bulma-layout` (`references/columns.md`).

## Sources
- https://bulma.io/documentation/start/responsiveness/ — the 5 breakpoints, exact pixel values, mobile-first model, 9 responsive mixins
- https://bulma.io/documentation/layout/container/ — container max-widths per breakpoint, `is-widescreen` / `is-max-desktop` / `is-max-widescreen` variants
