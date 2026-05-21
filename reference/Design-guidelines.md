# Design System Foundations & Components

This file contains the foundational tokens and structural component specifications for the UNI Design System. Use these definitions to ensure visual harmony, accessibility, and implementation consistency across all applications.

---

## 1. Typography

* **Primary Font Family:** `Inter` (used for headlines, body, and UI elements)
* **Monospace Font Family:** `JetBrains Mono` (used for code and technical data)
* **Paragraph Spacing:** Minimum of `1.4x` the line height between paragraphs

### Font Weights

| Token Name | Weight Value | Notes |
| :--- | :--- | :--- |
| `Bold` | 700 | Reserved for emphasis; available across all heading steps |
| `Medium` | 500 | Default weight for headings, field labels, button text |
| `Regular` | 400 | Default weight for body copy, paragraphs, input values |

### Text Styles

The UNI Design System publishes **two parallel scales**: a **Heading scale** with tight line-heights for single-line UI chrome (titles, labels, tabs), and a **Body scale** with looser line-heights for prose, inputs, and wrapping text. Style tokens follow the pattern `<scale>-<step>/font.<weight>` (e.g. `body-100/font.regular`). Body styles also have underline variants (`body-100/underline/font.medium`) reserved for inline links.

#### Heading Scale

For titles, labels, tabs, and any single-line UI element. Tight line-heights keep row alignment compact.

| Token | Size / LH | Confirmed Usage |
| :--- | :--- | :--- |
| `heading-600` | TBD | Hero / display heading *(defined in library; not observed in sampled screens)* |
| `heading-500` | TBD | Large statistic / page hero *(defined in library; not observed in sampled screens)* |
| `heading-400` | 24 / 32 | Page title (e.g. "Insights Dashboard", "Knowledge base") |
| `heading-300` | 18 / 24 | Settings section header, modal title (e.g. "Profile", "Add Item") |
| `heading-200` | 16 / 20 | Card / sub-section header, chart card title, contact-card label group |
| `heading-100` | 14 / 16 | Field label, tab label, list-row metadata label |
| `heading-050` | TBD | Smallest label *(defined in library; not observed in sampled screens)* |

Weight variants available for every step: `/font.medium` (default) and `/font.bold`.

#### Body Scale

For paragraphs, input values, button labels, helper text — any content that may wrap or sits inside form/data components.

| Token | Size / LH | Confirmed Usage |
| :--- | :--- | :--- |
| `body-100` | 14 / 20 | Body copy, input value, button label (default), radio/checkbox label, table cell |
| `body-050` | 12 / 16 | Helper text, small button label, table metadata ("3 items", "1 week ago"), chart legend |

Weight variants available for every step: `/font.regular` (default) and `/font.medium`. Underline variants (`body-XXX/underline/font.regular`, `body-XXX/underline/font.medium`) are reserved for inline links.

### Choosing Between Heading and Body at the Same Size

Two pairs of styles share the same font size but differ in line-height. The choice is driven by whether the text wraps and the role it plays:

| Size | Heading variant (tight LH) | Body variant (loose LH) |
| :--- | :--- | :--- |
| 14px | `heading-100` (LH 16) — tabs, field labels, sub-section title, single-line metadata | `body-100` (LH 20) — paragraphs, input content, button labels |
| 16px | `heading-200` (LH 20) — card titles, compact section heads | `body-200` (LH 22) — form sub-section titles, breathable headings |

**Rule of thumb:** if the text is a single-line UI element whose container relies on a fixed row height, use the **heading** variant. If the text may wrap, sits in a content block (input, paragraph, button), or is a form-section heading that breathes within a vertical stack, use the **body** variant.

---

## 2. Layouts

The platform supports two top-level layout modes. Choose based on whether the view depends on cross-tool navigation or directs the user into a single task.

### Default

Standard platform layout for all primary navigation-dependent views: tables, lists, dashboards, overviews. The Side Navigation is always visible, allowing the user to move freely across tools without losing context.

The Side Navigation has two states:

* **Expanded** *(default)* — Full nav with labels.
* **Collapsed** — Icon-only nav for users who want more horizontal room while keeping cross-tool access.

### Focus Mode

Removes the Side Navigation to reduce cognitive load and direct the user's full attention to a single task. Used whenever the complexity or nature of the task benefits from an uninterrupted, full-width surface.

Two variants:

* **Full Width** — Linear, step-by-step flows (wizards, multi-step journeys) and dense single-surface configurations. The user works through content sequentially or navigates a dense form without needing to reference other tools.
* **Split View** — Creation flows where the user configures something on the left and sees a live preview of the output on the right simultaneously. The setup surface is fixed-width; the preview surface grows with the viewport.

**Entry / exit rule.** Focus Mode is always entered from a Default view and must provide a clear exit path back to it — typically a close action in a local header at the top of the surface.

For structural specs (shell composition, grid, pane widths, header heights), see [`Layout.md`](./Layout.md).

---

## 3. Spacing Scale

Built on a consistent 4px increment system. Ensure interactive touch targets maintain a minimum size of 44x44px for accessibility.

| Token | Value | Primary Usage |
| :--- | :--- | :--- |
| `$space-0` | 0px | No spacing |
| `$space-025` | 2px | Minimal spacing, tight icon padding |
| `$space-050` | 4px | Component-level micro adjustments |
| `$space-100` | 8px | Internal element grouping / Small gaps |
| `$space-150` | 12px | Standard component padding / Intermediate gaps |
| `$space-200` | 16px | Container padding / Layout baselines |
| `$space-250` | 20px | Extended element spacing |
| `$space-300` | 24px | Structural section grouping |
| `$space-400` | 32px | Medium layout gutters |
| `$space-500` | 40px | Component block segregation |
| `$space-600` | 48px | Large content block division |
| `$space-700` | 56px | Sub-page layout grouping |
| `$space-800` | 64px | Major page canvas breaks |

---

## 4. Border Radius Scale

Controls the curvature of UI elements to define structural hierarchy and nesting principles.

| Token | Value | Primary Usage |
| :--- | :--- | :--- |
| `$radius-0` | 0px | Sharp corners, unrounded structural edges |
| `$radius-025` | 2px | Micro elements (checkboxes, tags) |
| `$radius-050` | 4px | Small components (tooltips, small buttons) |
| `$radius-075` | 6px | Medium components (standard buttons, inputs) |
| `$radius-100` | 8px | Large components (cards, dropdown menus) |
| `$radius-150` | 12px | Floating UI structural items (modals, popovers) |
| `$radius-200` | 16px | Large layout blocks / Nested container baselines |
| `$radius-300` | 24px | Distinct dashboard widgets / Highlight components |
| `$radius-400` | 32px | Outer layout framing layers |
| `$radius-500` | 40px | High-emphasis creative accent frames |
| `$radius-600` | 48px | Heavy macro curved panels |
| `$radius-700` | 56px | Specialty interface enclosures |
| `$radius-800` | 64px | Maximum system curvature bounds |

---

## 5. Elevation & Shadows

Shadows communicate depth and position on the Z-axis, structured from lowest to highest elevation.

| Token | Value / Definition | Target Usage |
| :--- | :--- | :--- |
| `$shadow-xs` | `0px 1px 2px 0px rgba(0, 0, 0, 0.05);` | Subtle depth for small inline elements |
| `$shadow-sm` | `0px 1px 3px 0px rgba(0, 0, 0, 0.10), 0px 1px 2px 0px rgba(0, 0, 0, 0.06);` | Light shadow for slightly elevated items |
| `$shadow-md` | `0px 4px 6px -1px rgba(0, 0, 0, 0.08), 0px 2px 4px -1px rgba(0, 0, 0, 0.06);` | Medium depth for elevated cards and widgets |
| `$shadow-lg` | `0px 10px 15px -3px rgba(0, 0, 0, 0.08), 0px 4px 6px -2px rgba(0, 0, 0, 0.05);` | Pronounced elevation for navigation or menus |
| `$shadow-xl` | `0px 20px 25px -5px rgba(0, 0, 0, 0.10), 0px 10px 10px -5px rgba(0, 0, 0, 0.04);` | Heavy shadow reserved for large floating panels |
| `$shadow-2xl` | `0px 25px 50px -12px rgba(0, 0, 0, 0.25);` | Highest native depth tier (use sparingly) |
| `$shadow-overlay` | `0px 0px 0px 1px rgba(0, 0, 0, 0.10), 0px 24px 48px 0px ...` | Blocking dialogs and root level Modals |

---

## 6. Colors

### Archetypes

The core color strategy is split into four distinct behavioral categories:

* **Brand:** Primary identity colors used for key actions, focus states, and core branding moments.
* **Semantic:** Functional indicators used strictly to communicate status (Success, Warning, Error, Info).
* **Neutral:** Foundation tones reserved for text variations, dividers, borders, and background canvases.
* **Accent:** High-contrast highlight colors applied sparingly to prioritize specific user interactions.

### Token Naming

All color tokens follow the pattern `color/<role>/<modifier>`.

* **Role** — *where* the color is applied. Six roles exist: `text`, `background`, `surface`, `border`, `divider`, `icon`.
* **Modifier** — *which variant* within that role: semantic emphasis (`primary`, `secondary`, `tertiary`), state (`disabled`, `selected`, `active`, `default`), tone (`bold`, `subtle`, `subtler`, `subtlest`), purpose (`brand`, `danger`, `success`, `warning`, `discovery`), or accent (`accent/red`, `accent/green`, `accent/blue`, `accent/teal`).

The same archetype reappears under different roles — e.g. the brand green is `color/background/brand/bold` (`#1ad678`) on a primary button, `color/border/selected` (`#19b868`) on an active tab, and `color/text/selected` (`#16a25b`) on an active nav label.

### Usage at a Glance

**Text** — Use `color/text/primary` (`#171717`) for headings and body copy; `color/text/secondary` (`#6b6b6b`) for sub-labels, helper text, and inactive states; `color/text/placeholder` (`#898989`) for input placeholders and disabled button labels; `color/text/selected` (`#16a25b`) for the active nav item. Inline status indicators inside metrics use `color/text/accent/green` (`#107a44`) and `color/text/accent/red` (`#ae2924`).

**Background & Surface** — `color/background/default` (`white`) for the page canvas; `color/surface/default` (`white`) for cards. Primary CTAs use `color/background/brand/bold` (`#1ad678`); secondary buttons use `color/background/neutral/subtlest` (`white`). Disabled surfaces fall back to `color/background/neutral/subtler` (`#f7f6f6`). The selected-row tint is `color/background/selected/subtle` (`#e8fcf3`). Chart fills and tag tints use the `color/background/accent/*/subtle` family — e.g. `#4ce599` (green) and `#f86d68` (red).

**Border & Divider** — `color/border/default` (`#e0e0e0`) for cards and input outlines; `color/border/selected` (`#19b868`) for the active-tab underline; `color/divider/default` (`#e0e0e0`) for in-card separators.

**Icon** — `color/icon/boldest` (`#171717`) for primary action icons; `color/icon/subtle` (`#6b6b6b`) for chevrons and helper glyphs; `color/icon/subtler` (`#c2c2c2`) for de-emphasized ornament; `color/icon/danger` (`#c9312c`) for destructive icons.

### Accessibility

> Standard text must preserve a minimum contrast ratio of `4.5:1` against its background. Large typography (18px bold / 24px regular) and graphic UI indicators require a minimum of `3:1`.

For the complete token list with all variants and hex values, see [`Color-tokens.md`](./Color-tokens.md).

---

## 7. Components

### Button

A fundamental interactive component used to trigger user actions or process navigation pathways.

#### 1. Anatomy
* **1. Container:** Defines button boundaries, padding, and shape.
* **2. Leading Icon:** Optional layout element placed before the label text.
* **3. Label:** Center text string wrapped properly based on boundaries.
* **4. Trailing Icon:** Optional layout element placed after the label text.

#### 2. Layout & Size System
* **Large:** Designed for prominent main page calls-to-action.
* **Medium:** The standardized sizing tier optimized for most basic layout variants.
* **Small:** Intended for compact structures, utility toolbars, or spaces with constrained real estate.

#### 3. Hierarchy Rules
* **Primary:** Core application CTA. Use exactly **one** primary button per context panel to explicitly dictate the user's primary forward action.
* **Secondary:** Cornerstone utility action. Uses a white surface container paired with a gray border to ensure visibility over flat layouts.
* **Tertiary:** Low emphasis interaction layer. Reserved predominantly for passive options like skipping, closing, or rejecting prompts.
* **Destructive:** Used strictly when executing irreversible system logic (e.g., permanent deletion of data).

#### 4. Interaction States & Baseline Values

##### Primary Button Tones
* **Default State**
  * Container Base: `#1ad678` (`$color-green-400`)
  * Text Label Base: `#171717` (`$color-neutral-900`)
  * Icon Graphic Base: `#171717` (`$color-neutral-900`)
* **Hover State**
  * Container Surface: `#18c36d` (`$color-green-500`)
* **Pressed State**
  * Container Surface: `#16b163` (`$color-green-600`)
* **Disabled State**
  * Container Surface: `#f7f6f6` (`$color-neutral-50`)
  * Text & Icon Color: `#898989` (`$color-neutral-500`)

##### Secondary Button Tones
* **Default State**
  * Container Base: `#ffffff` (`$color-neutral-0`)
  * Border Edge Base: `#e0e0e0` (`$color-neutral-200`)
  * Text Label Base: `#171717` (`$color-neutral-900`)
* **Hover State**
  * Container Surface: `#f7f6f6` (`$color-neutral-50`)
  * Border Edge: `#c2c2c2` (`$color-neutral-300`)
* **Pressed State**
  * Container Surface: `#efefef` (`$color-neutral-100`)

##### Destructive Button Tones
* **Default State**
  * Container Base: `#e2423d` (`$color-red-500`)
  * Text Label Base: `#ffffff` (`$color-neutral-0`)
* **Hover State**
  * Container Surface: `#c9312c` (`$color-red-600`)
* **Pressed State**
  * Container Surface: `#ae2924` (`$color-red-700`)