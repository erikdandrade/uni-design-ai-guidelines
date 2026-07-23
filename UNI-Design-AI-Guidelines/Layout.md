# Platform Layout

## Build Context

Everything in this file describes the **Unifonic Platform shell** — the navigation chrome that wraps a feature living *inside* the platform. Before applying it, settle which of two build contexts you are in (the AI builder asks the user up front — see `AI-builder-prompt.md` → *Build context*):

- **Platform-embedded** — the feature lives inside the Unifonic Platform. The full shell applies: Top Bar (Main Top Bar / Local Header swap pair), the `uni-menu` Side Navigation populated with the documented Console content map, the Content Area grid, and the Layout Modes below.
- **Standalone** — a tool that is **not** part of the platform's navigation. The platform chrome is **skipped**: no Main Top Bar, no Local Header, no Layout Modes, and no documented Console/Section content map. The tool lays out on its own surface. The `uni-menu` Side Navigation is **optional** — reuse it as a component only if the tool needs its own navigation, filled with the tool's own items (see [`Side-navigation.md`](./Side-navigation.md)). Foundations and components (typography, color, spacing, radius, elevation, Button, Containers, etc.) still apply in full — a standalone tool should still look unmistakably like Unifonic.

The remainder of this file (Shell, Top Bar, Side Navigation placement, Content Area, Layout Modes, Split View) is **platform-embedded** structure. A standalone build uses only the foundations and the component specs it chooses to reuse.

---

## Shell

The platform shell is a full-viewport flex column. It contains two children stacked vertically: the Top Bar and a horizontal flex row (Panel + Content).

```
shell
├── Top Bar
└── Panel + Content (flex row)
    ├── Side Navigation
    └── Content Area
```

The shell has no scrolling. Individual sections inside the Content Area may scroll independently.

The shell's base background uses `color/surface/sunken/default` (`#f7f6f6`). This sunken layer sits behind all content surfaces, creating a subtle depth separation between the page background and elevated elements (cards, panels, forms, Side Navigation). It is never used as a foreground or interactive element — its sole purpose is to establish the lowest visual layer of the layout.

---

## Top Bar

The shell's top row holds exactly **one** header, chosen by layout mode:

- **Default** mode (Expanded or Collapsed nav) → the **Main Top Bar** (`uni-top-bar`).
- **Focus Mode** (both **Full Width** and **Split View**) → the **Local Header** (`uni-local-header`), which **replaces** the Main Top Bar.
- **Embedded apps** (Agent Console, Chatbot) → **neither**; the app takes over the full screen with its own shell.

Common to both surfaces:

- Full viewport width.
- Height: `72px`. Fixed. Does not change across modes or breakpoints.

For the header **components** themselves — anatomy, slots, prop-toggled sections, tokens, and the full visibility matrix — see [`Top-bar.md`](./Top-bar.md). This file specifies only their placement within the shell.

---

## Side Navigation

- Rendered as the first child of the Panel + Content flex row.
- Height: viewport height minus `72px` (fills remaining shell height).
- Width is determined by the active layout mode (see Layout Modes).
- When width changes, it **pushes** the Content Area. It does not use `position: absolute` or overlay the Content Area.

For the navigation **component** itself — anatomy, states, the Console → Section → child content map, and rail/accordion behavior — see [`Side-navigation.md`](./Side-navigation.md). This file specifies only the nav's placement and width within the shell.

---

## Content Area

- Rendered as the second child of the Panel + Content flex row.
- Takes all remaining horizontal space (`flex: 1`).
- Contains a 12-column grid. Grid properties:

### ≤ 1440px viewport width

| Property | Value |
|---|---|
| Columns | 12 |
| Column width | Fluid (equal share of available width after gutters and margins) |
| Gutter | `24px` (fixed) |
| Margin (left + right) | `32px` (fixed) |

### > 1440px viewport width

| Property | Value |
|---|---|
| Columns | 12 |
| Column width | `76px` (fixed) |
| Gutter | `24px` (fixed) |
| Margin (left + right) | Fluid (absorbs all remaining space, centers the content block) |

At `>1440px`, total content block width is: `(12 × 76px) + (11 × 24px)` = `912px + 264px` = `1176px`. The margin fills the rest.

Page content within the Content Area is wrapped in **`uni-box`** (default — sections without an explicit save action) or **`uni-form`** (when changes must be saved as a group before taking effect). See `Design-guidelines.md` § 7 Components → Containers for the full specification.

---

## Layout Modes

The platform has two top-level layout modes — **Default** and **Focus Mode** — selected based on whether the view supports cross-tool navigation or directs the user into a single task.

### Default

Used for all primary navigation-dependent views: tables, lists, dashboards, overviews. The Side Navigation is always visible, allowing the user to move freely across tools without losing context.

The Side Navigation has two states:

#### Expanded *(default)*

| Element | Value |
|---|---|
| Main Top Bar | Visible |
| Side Navigation | Visible, width `200px` |
| Content Area | `flex: 1`, remaining width after `200px` nav |

#### Collapsed

| Element | Value |
|---|---|
| Main Top Bar | Visible |
| Side Navigation | Visible, width `40px` (icon rail) |
| Content Area | `flex: 1`, remaining width after `40px` nav |

### Focus Mode

Removes the Side Navigation to reduce cognitive load and direct the user's full attention to a single task. Used whenever the complexity or nature of the task benefits from an uninterrupted, full-width surface.

**Entry / exit rule.** Focus Mode is always entered from a Default view and must provide a clear exit path back to it — typically a close action in the local header at the top of the surface.

Two variants:

#### Full Width

For linear, step-by-step flows (wizards, multi-step journeys) and dense single-surface configurations. The user works through content sequentially or navigates a dense form without needing to reference other tools.

| Element | Value |
|---|---|
| Main Top Bar | Not rendered |
| Local Header | Visible — replaces the Main Top Bar |
| Side Navigation | Not rendered |
| Content Area | `flex: 1`, full viewport width |

#### Split View

For creation flows where the user configures something on the left (Setup Pane) and sees a live preview on the right (Preview Pane) simultaneously. The Setup Pane is fixed-width; the Preview Pane grows with the viewport. The platform Top Bar is replaced by a Local Header in this variant.

See **Split View Layout** below for the full structural specification (shell, Local Header, panes, grids).

---

## Content Rules

- All content inside the Content Area must be sized in **column spans**, not fixed pixel widths.
- Column widths are fluid at `≤1440px` and will change as the Side Navigation expands or collapses. Content must reflow correctly in both nav states.
- At `>1440px`, column widths are fixed at `76px`. Do not override this with fluid sizing above this breakpoint.
- Gutters (`24px`) and margins (`32px` fixed, fluid above `1440px`) must never be collapsed or overridden by content.
- Do not assume a specific Side Navigation state when sizing content. A component that spans 8 columns must work correctly at `200px` nav, `40px` nav, and no nav.

---

## Split View Layout

A variant of Focus Mode for creation and configuration flows. The user configures on the left (Setup Pane) and previews the result on the right (Preview Pane) simultaneously. The Side Navigation is never present in this mode.

### Shell Structure

```
shell
├── Local Header
└── Split Row (flex row, full remaining height)
    ├── Setup Pane (left)
    └── Preview Pane (right)
```

### Local Header

- Replaces the Main Top Bar in this mode (and in Focus Mode → Full Width).
- Full viewport width.
- Height: `72px`. Fixed.
- Left side: close (`×`) action + flow title (the required Focus-Mode exit path).
- Right side: action slot — default is a Secondary (cancel) + Primary (submit) pair.
- Do not render the Main Top Bar when the Local Header is active.

For the component's full anatomy, slots, and tokens, see [`Top-bar.md`](./Top-bar.md) → `uni-local-header`.

### Split Row

- Horizontal flex row.
- Height: viewport height minus `72px`. No scrolling at row level.
- Each pane scrolls independently.

### Setup Pane (left)

- `flex: 0 0 auto`. Does not grow beyond its column-defined width.
- Scrollable vertically (`overflow-y: auto`). Clips horizontally.
- Right border divider separating it from the Preview Pane.
- 6-column grid:

| Property | ≤ 1440px | > 1440px |
|---|---|---|
| Columns | 6 | 6 |
| Column width | Fluid | `76px` (fixed) |
| Gutter | `24px` | `24px` |
| Margin (left + right) | `32px` | `32px` |

At `≤1440px`: pane width = `50%` of viewport = `720px`. Columns are fluid within it.

At `>1440px`: pane width is fixed at `(6 × 76px) + (5 × 24px) + (2 × 32px)` = `640px`. Does not grow further.

**Composition rule.** The Setup Pane content is laid out **directly on the pane surface**. Do **not** wrap it in a `uni-box` or `uni-form` — the pane itself provides the bounded surface that a card would normally supply, and an additional inner card would create a redundant nested container. The primary commit action and the cancel / exit action hoist to the **Local Header** (Submit on the right, close `×` on the left); the Setup Pane therefore has no `uni-form-footer` of its own.

### Preview Pane (right)

- `flex: 1`. Takes all remaining width at every viewport size.
- At `≤1440px`: `720px` (mirrors Setup Pane).
- At `>1440px`: grows fluidly as viewport expands. Setup Pane does not grow with it.
- 6-column grid:

| Property | ≤ 1440px | > 1440px |
|---|---|---|
| Columns | 6 | 6 |
| Column width | Fluid | Fluid (absorbs extra width as pane grows) |
| Gutter | `24px` | `24px` |
| Margin (left + right) | `64px` | `64px` (fixed) |

- Preview card is centered within the pane using the `64px` margin. It spans all 6 columns and grows with the pane above `1440px`.

### Split View Rules

- All content in the Setup Pane must use column spans (1–6). Do not use fixed pixel widths.
- Do not assume Setup Pane width is `720px`. At `>1440px` it is `640px`. Use 6-column spans, not pixel containers.
- The Preview Pane must not use fixed widths for the preview card. It spans 6 columns and grows with the pane.
- The two panes never overlap. No `position: absolute` relationship between them.
- Side Navigation is never rendered in this mode. Do not reserve space for it.
