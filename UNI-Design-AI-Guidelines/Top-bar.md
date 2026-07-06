# uni-top-bar & uni-local-header — Platform Headers

Specification for the UNI Design System's two top-of-shell surfaces. Authored from the live Figma source (file: *UNI Design System*; `uni-top-bar` node `47959:147152`, `uni-local-header` node `54172:3214`). Intended for LLM-assisted UI generation (e.g. Lovable). Read this whole file before generating a header; the two surfaces are a **swap pair** — exactly one of them renders at the top of the shell, never both.

> **Where this fits.** Both surfaces are the top row of the platform shell. For the shell composition, height, and how the row sits above the Side Navigation + Content row, see [`Layout.md`](./Layout.md) → *Shell*, *Top Bar*, and *Layout Modes*. This file specifies the **components themselves** — anatomy, slots, states, and tokens.

---

## 1. Which surface renders (visibility matrix)

The shell's top row holds **one** header. Which one depends on the layout mode. This is the single most important rule — the header is not free-form furniture.

| Layout context | Top-row surface |
|---|---|
| **Default** — Expanded nav (200px) | **Main Top Bar** (`uni-top-bar`) |
| **Default** — Collapsed nav (40px rail) | **Main Top Bar** (`uni-top-bar`) |
| **Focus Mode → Full Width** | **Local Header** (`uni-local-header`) — replaces the Main Top Bar |
| **Focus Mode → Split View** | **Local Header** (`uni-local-header`) — replaces the Main Top Bar |
| **Embedded apps** (Agent Console, Chatbot) | **Neither.** Opening the app redirects to a full-screen standalone app with its own shell. No platform header is rendered. |

**Rules:**

1. The Main Top Bar shows **only** in Default mode. The moment the user enters Focus Mode (either variant), it is replaced by the Local Header.
2. Never render both at once, and never stack one below the other.
3. Embedded apps (Agent Console, Chatbot) are outside the platform shell entirely — do not reserve space for, or render, either header inside them.

Both surfaces are `72px` tall, full viewport width, on `color/surface/default` (`#ffffff`). They differ only in edge treatment: the Main Top Bar carries `Shadow/xs`; the Local Header carries a bottom border.

---

## 2. Tokens and constants

**Color tokens — resolved hex.** For vibe-coding, use the raw **Light-mode hex** as the build value. Token names identify *what a color is for*; the build does not consume the token structure.

| Token | Role | Light (build value) |
|---|---|---|
| `color/surface/default` | Header surface | `#ffffff` |
| `color/border/default` | Section dividers, bottom border, button borders | `#e0e0e0` |
| `color/text/primary` | Section label (top line) | `#171717` |
| `color/text/secondary` | Section value (bottom line), Local Header title | `#6b6b6b` |
| `color/icon/boldest` | Functional icons (wallet, globe) | `#171717` |
| `color/icon/subtle` | Chevrons, user-circle, close | `#6b6b6b` |
| `color/icon/danger` | Impersonation stop-circle | `#c9312c` |
| `color/background/neutral/subtlest` | Top Up / secondary button surface | `#ffffff` |

**Effect:** `Shadow/xs` — drop shadow `0px 1px 2px rgba(0,0,0,0.05)` (Main Top Bar only).

**Sizing constants (px):**

| Constant | Value |
|---|---|
| Header height (both surfaces) | 72 |
| Main Top Bar edge | `Shadow/xs` (no border) |
| Local Header edge | 1px bottom border, `color/border/default` |
| Section padding (Main Top Bar) | 12 (`space/150`) |
| Section divider | 1px left border, `color/border/default` |
| Gap: icon → text within a section | 16 (`space/200`) |
| Local Header horizontal padding | 16 (`space/200`) |
| Local Header `.heading` gap | 8 (`space/100`) |
| Local Header action slot gap | 8 (`space/100`) |
| Functional icon slot | 16 × 16 |
| Chevron / user-circle icon slot | 16 × 16 |
| Close icon slot (Local Header) | 24 × 24 |
| Top Up button height | 32 |

**Typography:**

| Use | Style | Size / line-height | Weight |
|---|---|---|---|
| Section label (top line) | `heading-100/font.medium` | 14 / 16 | Medium (500) |
| Section value (bottom line) | `body-100/font.regular` | 14 / 20 | Regular (400) |
| Local Header title | `heading-200/font.medium` | 16 / 20 | Medium (500) |
| Top Up button label | `body-050/font.medium` | 12 / 16 | Medium (500) |

Icons are **Font Awesome** glyphs rendered as text nodes (`stop-circle`, `angle-down`, `wallet`, `globe`, `user-circle`, `close`), not SVG assets. The left logo is the exception (§3.1).

---

## 3. `uni-top-bar` — Main Top Bar

Container: `flex` row, `items-center`, `justify-between`, full width, height `72px`, surface `#ffffff`, `Shadow/xs`, `overflow: clip`. Two regions: **Logo** (left) and **Actions** (right).

### 3.1 Logo (left)

The **UNIFONIC wordmark** (green), rendered as an inline SVG/image asset (~132 × 18px) inside a padded box. This is the brand logo — **not** a letter-avatar square, and **not** the word "UNI Platform". Do not substitute a generic app name or a colored initial tile.

### 3.2 Actions (right)

A horizontal row of **Sections**, right-aligned, full header height. Each section is divided from the previous by a **1px left border** (`color/border/default`), has `12px` padding, and lays out an icon + a two-line text block with a `16px` gap. Sections are toggleable via boolean props (all default `true` except where noted); order left-to-right:

| # | Section | Prop | Icon | Icon color | Top line (label) | Bottom line (value) | Extra |
|---|---|---|---|---|---|---|---|
| 1 | Impersonation | `impersonation` | `stop-circle` | `icon/danger` `#c9312c` | "Stop" | "Impersonating" | Conditional — render **only while impersonating**. Both lines use `body-100/regular`, `text/primary`. First section → no left divider. |
| 2 | Packages | `packages` | — | — | "Packages" | "3 active" | Trailing `angle-down` chevron (`icon/subtle`), opens a picker. |
| 3 | Balance | `balance` | `wallet` | `icon/boldest` `#171717` | "Balance" | "USD 330K" | Optional **Top Up** button (`topUp`, default `true`) — 32px, secondary/compact: surface `#ffffff`, 1px border `#e0e0e0`, radius `6px` (`radius/075`), label `body-050/medium` `#171717`. |
| 4 | Timezone | `timezone` | `globe` | `icon/boldest` `#171717` | "Timezone" | "Asia/Riyadh" | — |
| 5 | Account menu | *(always)* | `user-circle` | `icon/subtle` `#6b6b6b` | "UserName" | "Account name" | Trailing `angle-down` chevron (`icon/subtle`), opens the account/user menu. Always present. |

**Text pattern (sections 2–5):** top line = label in `heading-100/font.medium` (`text/primary` `#171717`), bottom line = value in `body-100/font.regular` (`text/secondary` `#6b6b6b`).

### 3.3 Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `impersonation` | BOOLEAN | `true` | Show the Impersonation section (render only when actually impersonating). |
| `packages` | BOOLEAN | `true` | Show the Packages section. |
| `balance` | BOOLEAN | `true` | Show the Balance section. |
| `topUp` | BOOLEAN | `true` | Show the Top Up button inside Balance. |
| `timezone` | BOOLEAN | `true` | Show the Timezone section. |
| `type` | VARIANT | `Default` | Only `Default` is defined. |

The account menu has no toggle — it is always rendered.

---

## 4. `uni-local-header` — Local Header

Container: `flex` row, `items-center`, `justify-between`, full width, height `72px`, surface `#ffffff`, **1px bottom border** (`color/border/default`), horizontal padding `16px`. Replaces the Main Top Bar throughout Focus Mode.

### 4.1 Anatomy

```
uni-local-header (horizontal, justify-between, px 16, h 72)
├─ .heading (horizontal, gap 8)
│  ├─ uni-button-compact          ← close (×) action, "close" glyph, icon/subtle #6b6b6b, 24px slot
│  └─ Header (horizontal, gap 4)
│     ├─ Title Header (TEXT 16px) ← heading-200/medium, text/secondary #6b6b6b
│     └─ uni-button-icon          ← trailing tertiary icon button (e.g. context/more), 24px
└─ .wrap (horizontal, gap 8, px 16)   ← action slot; toggleable via showSlot
   └─ [slot] default = Secondary button + Primary button   ← e.g. Cancel + Save/Submit
```

### 4.2 Slots and properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `showSlot` | BOOLEAN | `true` | Show the right-side action slot. |
| *(slot)* `children` | SLOT | Secondary + Primary buttons | The right-side actions. Apps inject their own; the default is a Secondary (cancel / discard) + Primary (save / submit) pair. |

**Left side** carries the exit path (close `×`) and the flow title — this is the required exit affordance for Focus Mode (see `Layout.md` → *Focus Mode* entry/exit rule). **Right side** carries the flow's commit actions. In Split View, the Setup Pane has no footer of its own — its Submit / cancel actions hoist here (see `Layout.md` → Split View → Setup Pane).

---

## 5. Implementation notes (read before generating)

1. **One header, chosen by mode.** Render the Main Top Bar in Default mode and the Local Header in Focus Mode — never both, never neither (except embedded apps, which render neither). (§1)
2. **Do not invent top-bar contents.** The Main Top Bar has a fixed anatomy: UNIFONIC wordmark + the five right-side sections in §3.2. It has **no global search field, no help / notification / settings icon cluster, and no letter-avatar tile.** If a product needs one of those, that is a gap to flag — do not add it silently.
3. **Two-line sections.** Each right-side section is a label (medium, primary) over a value (regular, secondary), not a single string. Keep the 16px icon→text gap and the 1px left divider between sections.
4. **Icons are Font Awesome text glyphs.** Except the UNIFONIC logo, which is an inline image/SVG asset. Don't render section icons as SVG assets or the logo as a glyph.
5. **Height is 72px for both surfaces.** The Local Header is also 72px (matches the Main Top Bar) — not 65px.
6. **Edge treatment differs.** Main Top Bar = `Shadow/xs`, no border. Local Header = 1px bottom border, no shadow.
7. **The Local Header is the Focus-Mode exit path.** Its left `close (×)` + title is required; its right slot holds the commit actions. Don't drop the close action.
