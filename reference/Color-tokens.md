# Color Tokens

Canonical token list for the UNI Design System color palette, sourced from the Figma library `UNI Design System`. Usage philosophy lives in [`Design-guidelines.md`](./Design-guidelines.md#5-colors); this file is the lookup table.

Every entry includes the token name (for semantic reference) and a hex value (for direct color application when token context is not available). Tokens defined in the library but not yet observed in product screens are listed with `TBD` in the Hex column and `—` in Observed Usage.

---

## Text

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/text/primary` | `#171717` | Page titles, body copy, input values |
| `color/text/secondary` | `#6b6b6b` | Sub-labels, helper text, inactive tabs, chart axis labels |
| `color/text/tertiary` | TBD | — |
| `color/text/placeholder` | `#898989` | Input placeholders, disabled button labels |
| `color/text/disabled` | TBD | — |
| `color/text/brand` | TBD | — |
| `color/text/selected` | `#16a25b` | Active nav item label |
| `color/text/inverse` | TBD | — |
| `color/text/danger` | TBD | — |
| `color/text/success` | TBD | — |
| `color/text/warning` | TBD | — |
| `color/text/discovery` | TBD | — *(product meaning to confirm)* |
| `color/text/accent/red` | `#ae2924` | Inline danger-tone metric (e.g. "Closed Sessions") |
| `color/text/accent/green` | `#107a44` | Inline success-tone metric (e.g. "Open Sessions") |
| `color/text/accent/blue` | TBD | — |
| `color/text/accent/teal` | TBD | — |

---

## Background

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/background/default` | `white` | Page canvas, modal body |
| `color/background/inverse` | TBD | — |
| `color/background/disabled` | TBD | — |
| `color/background/input/default` | `white` | Form input fields |
| `color/background/input/active` | TBD | — |
| `color/background/brand/bold` | `#1ad678` | Primary CTA button |
| `color/background/brand/subtle` | TBD | — |
| `color/background/neutral/bold` | TBD | — |
| `color/background/neutral/subtler` | `#f7f6f6` | Disabled button surface |
| `color/background/neutral/subtlest` | `white` | Secondary button surface |
| `color/background/selected/bold` | TBD | — |
| `color/background/selected/subtle` | `#e8fcf3` | Selected nav item background tint |
| `color/background/success/bold` | TBD | — |
| `color/background/danger/bold` | TBD | — |
| `color/background/warning/bold` | TBD | — |
| `color/background/accent/red/bold` | TBD | — |
| `color/background/accent/red/subtle` | `#f86d68` | Chart bars — closed sessions |
| `color/background/accent/green/subtle` | `#4ce599` | Chart bars — open sessions |
| `color/background/accent/blue/bold` | TBD | — |

---

## Surface

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/surface/default` | `white` | Card surface, chart card |
| `color/surface/sunken/default` | `#f7f6f6` | Sunken / nested container background |

---

## Border

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/border/default` | `#e0e0e0` | Card borders, secondary button borders |
| `color/border/input/default` | `#e0e0e0` | Form input borders |
| `color/border/selected` | `#19b868` | Active-tab underline |
| `color/border/inverse` | `#474747` | Dark / inverse-surface border |

---

## Divider

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/divider/default` | `#e0e0e0` | In-card section dividers |

---

## Icon

| Token | Hex | Observed Usage |
| :--- | :--- | :--- |
| `color/icon/boldest` | `#171717` | Primary action icons |
| `color/icon/subtle` | `#6b6b6b` | Chevrons, helper glyphs |
| `color/icon/subtler` | `#c2c2c2` | De-emphasized ornament |
| `color/icon/danger` | `#c9312c` | Destructive action icons |

---

## Notes

* **`color/text/discovery`** — defined in the library; product meaning is still TBD. Likely intended for "discovery" features (commonly a purple in similar systems). Confirm with the design system owner before adopting.
* **Parallel `Test UNI Design System` library** — Figma also publishes a parallel library with a flatter naming scheme (`color/brand/primary`, `color/feedback/error`, etc.). It is **not** the source of truth for production screens and tokens from it should not be referenced in product code. If those names appear in a search, treat them as stale duplicates.
* **`TBD` hex values** — tokens exist in the library but were not observed in the "Examples for AI" wrap of sampled screens. Update this file with the canonical hex the first time the token is used in product, and add a one-line description in Observed Usage.
