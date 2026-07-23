# uni-menu — Side Navigation

Specification for the UNI Design System side navigation. Authored from the live Figma source (file: *UNI Design System*, page: *Side Menu*). Intended for LLM-assisted UI generation (e.g. Lovable). Read this whole file before generating; the implementation gotchas at the end prevent the most common mistakes.

> **Where this fits.** The side navigation is the `Side Navigation` element of the platform shell. For its position, height, and how its width pushes the Content Area, see [`Layout.md`](./Layout.md) → *Side Navigation* and *Layout Modes*. This file specifies the **component itself** — anatomy, states, content map, and icons.
>
> **Build context.** The component anatomy, states, sizing, and icon policy (§1–§3, §6–§7) apply in **both** build contexts (see `Layout.md` → *Build Context*). The **Console → Section → child content map** in §5.4, however, is **platform-embedded only** — it is the platform's own navigation structure. In a **standalone** build the side nav is optional; if used, reuse the component with the **tool's own sections and items**, not this content map.
>
> **Icon assets.** The 20 section logos referenced throughout this file live in [`./menu-icons/`](./menu-icons/); the Console/Section → file mapping is in [`./menu-icons/MANIFEST.md`](./menu-icons/MANIFEST.md). All are `currentColor` SVGs that inherit the menu-item text color.

---

## 1. Component hierarchy

The navigation is one public component composed of three private internal parts. Build **from `uni-menu`**. Do not instantiate the dotted components directly — they are internal anatomy.

```
uni-menu              ← PUBLIC. The full side nav. Build from this.
└─ .section           ← private. A parent menu-item + its child menu-items (an accordion group).
   └─ .uni-menu-item  ← private. A single row (leading slot + label + trailing chevron).
.Base Menu Item       ← private. Used only for the account Switcher header (see §5.2).
uni-separator         ← instance. 1px divider between section groups.
```

A leading dot (`.`) marks a component hidden from the published library. It signals "internal — compose, don't place." Treat `.uni-menu-item`, `.section`, and `.Base Menu Item` as build-time anatomy only.

---

## 2. Tokens and constants

**Color tokens — resolved hex.** For vibe-coding, use the raw **Light-mode hex** below as the build value. The token name is kept only to identify *what a color is for*; the build does not consume the token structure. Each token aliases to a primitive (shown for traceability).

| Token | Role | Light (build value) | Dark | Primitive (Light) |
|---|---|---|---|---|
| `color/text/primary` | Default label text | `#171717` | `#ffffff` | `color/neutral/900` |
| `color/text/secondary` | Hover label text | `#6b6b6b` | `#ababab` | `color/neutral/600` |
| `color/text/selected` | Selected label (green active) | `#16a25b` | `#1ad678` | `color/green/600` |
| `color/icon/boldest` | Label text in Pressed / Expanded (intended — see §3.3) | `#171717` | `#ebebeb` | `color/neutral/900` |
| `color/icon/subtle` | Trailing chevron, all states | `#6b6b6b` | `#ababab` | `color/neutral/600` |

> **Modes:** these tokens carry Light and Dark modes. The Light column is the default nav rendering. If the build is light-only, ignore the Dark column. If it themes, carry both.
>
> **Light-mode collision (by design):** in Light mode `color/icon/boldest` and `color/text/primary` are both `#171717`. So a light-only build renders the Pressed/Expanded label identically to Default — the distinction is real but only visible in Dark mode. This is expected, not a bug.

**Sizing constants (px):**

| Constant | Value |
|---|---|
| Row height | 24 |
| Leading icon slot | 24 × 24 (always reserved — see §6.1) |
| Icon glyph box inset | 3 (vector renders at 18×18 inside the 24 slot) |
| Label font size | 14 |
| Chevron font size | 12 |
| Gap: leading slot → label container | 8 |
| Gap: label → trailing chevron | 8 |
| Label container horizontal padding | 4 left / 4 right |
| Expanded nav width | 200 |
| Collapsed (rail) nav width | 40 |
| Section-group inner padding | 8 on all sides |
| Panel vertical padding | 8 top / 8 bottom, 0 horizontal |
| Gap between groups / items in Panel | 8 |
| Separator | 1px high, full width |

---

## 3. `.uni-menu-item` — a single row

The atomic row. One leading icon slot, a label, and an optional trailing chevron.

### 3.1 Anatomy

```
.uni-menu-item (vertical wrapper)
└─ container (horizontal, gap 8, align: start / center)
   ├─ Icons container (24×24)        ← leading slot; holds the Logo instance
   │  └─ Logo (INSTANCE_SWAP)        ← product logo, inline SVG (see §3.4)
   └─ container (horizontal, gap 8, padding 0/4/0/4)
      ├─ Menu item (TEXT, 14px)      ← the label
      └─ Icons (INSTANCE)            ← trailing chevron
         └─ icon (TEXT, 12px)        ← FontAwesome glyph (see §3.4)
```

### 3.2 Properties

| Property | Type | Default | Purpose |
|---|---|---|---|
| `Text` | TEXT | `"Menu item"` | Label string |
| `Left Logo` | BOOLEAN | `true` | Show the leading product logo |
| `Right icon` | BOOLEAN | `true` | Show the trailing chevron |
| `Logo` | INSTANCE_SWAP | `admin-logo` | Which product logo fills the leading slot (~28 options) |

### 3.3 Variant axes and state → token map

Two variant axes: **`State`** (Default, Hover, Pressed, Expanded, Selected) × **`Collapsed`** (False, True) = 10 variants. Here `Collapsed=True` means **rail / icon-only mode** (row shrinks to 24×24, label and chevron hidden). This is a different concept from `.section`'s `Collapsed` — see §7.

State behavior (non-collapsed form). Hex shown is Light-mode build value:

| State | Label color | Label hex | Trailing chevron | Chevron color | Chevron hex |
|---|---|---|---|---|---|
| Default | `color/text/primary` | `#171717` | `angle-down` | `color/icon/subtle` | `#6b6b6b` |
| Hover | `color/text/secondary` | `#6b6b6b` | `angle-down` | `color/icon/subtle` | `#6b6b6b` |
| Pressed | `color/icon/boldest` | `#171717` | `angle-down` | `color/icon/subtle` | `#6b6b6b` |
| Expanded | `color/icon/boldest` | `#171717` | `angle-up` | `color/icon/subtle` | `#6b6b6b` |
| Selected | `color/text/selected` | `#16a25b` | `angle-up` | `color/icon/subtle` | `#6b6b6b` |

**Intended, not a bug:** in Pressed and Expanded the label text binds to `color/icon/boldest` (an icon-ramp token applied to text). This is deliberate — label and icon share the boldest ramp in active states. Do not "correct" it to a `color/text/*` token. Likewise the chevron stays `color/icon/subtle` in every state including Selected; only the label and leading logo adopt the active color, the chevron stays neutral.

The chevron flips `angle-down → angle-up` when the item is Expanded or Selected (i.e. when its child section is open).

### 3.4 Icon policy

Two different icon mechanisms in one row — implement them differently:

- **Leading logo** = product/section logo, supplied via the `Logo` INSTANCE_SWAP. These are **inline SVG** component instances (e.g. Flow Studio, Campaigns, Admin). Render as SVG, not as a font glyph.
- **Trailing chevron** = a **FontAwesome (FA7) functional glyph rendered as a text node** (`angle-down` / `angle-up`, 12px). Render as an icon-font character, not as an SVG asset.

---

## 4. `.section` — a parent item + its children (accordion group)

A vertical stack of `.uni-menu-item` instances: index 0 is the parent (has a leading logo and a chevron), the rest are children (no leading logo, no chevron).

### 4.1 Anatomy

```
.section (vertical, gap 0)
├─ .uni-menu-item  (parent)   Left Logo=true,  Right icon=true,  State=Expanded
├─ .uni-menu-item  (child)    Left Logo=false, Right icon=false, State=Default
├─ .uni-menu-item  (child)    Left Logo=false, Right icon=false, State=Default
└─ … one .uni-menu-item per child route
```

Example — `Flow Studio`, expanded (height 160 = 1 parent + 4 children × 24, gap 0):
parent `Flow Studio`, children `Dashboard`, `My flows`, `Templates`, `Executions`.

### 4.2 Properties

Three variant axes:

| Axis | Values |
|---|---|
| `Console` | `User`, `Admin` |
| `Section` | `Flow Studio`, `Multichannel Campaigns`, `Chatbot`, `Integrations`, `Authenticate`, `uLink`, `Notice`, `Reports&Logs`, `Admin`, `Channels`, `Library`, `Developers`, `Audiences`, `Admin Management`, `Customers`, `Services`, `Reporting`, `Business Operations`, `Products`, `AI`, `Creative` |
| `Collapsed` | `True` (accordion closed — parent only), `False` (accordion open — parent + children) |

`Console` selects which console's sections are available. `Section` picks the named section (each carries its own logo and child set). `Collapsed` here is the **accordion** state, not rail mode (§7).

### 4.3 Parent vs child alignment rule

Children align under the parent label **because the 24px leading icon slot is always reserved, even when empty** — not because of added padding.

- Parent: `Left Logo=true` → slot renders the product logo.
- Child: `Left Logo=false` → slot is empty **but still occupies 24px**.
- The `gap: 8` after the reserved slot lands every label at the same x.

**Implement as:** always lay out a fixed 24px leading slot; render the logo into it only when `Left Logo` is on. Never implement children as "no icon + left-padding" — the padding will drift from the parent's icon width.

---

## 5. `uni-menu` — the full navigation

### 5.1 Properties

Two variant axes = 4 variants:

| Axis | Values |
|---|---|
| `State` | `Default` (200px), `Collapsed` (40px rail) |
| `Console` | `User`, `Admin` |

### 5.2 Anatomy

```
uni-menu (vertical)
└─ Panel (vertical, gap 8, padding 8/0/8/0)
   ├─ Switcher (62px, padding 8)        ← account/subaccount header
   │  └─ .Base Menu Item                ← e.g. "Marketing / Subaccount" + up/down toggle
   ├─ uni-separator (1px)
   ├─ [section group] (vertical, gap 8, padding 8)   ← N × .section
   ├─ uni-separator (1px)
   ├─ [section group] (vertical, gap 8, padding 8)   ← N × .section
   ├─ uni-separator (1px)
   ├─ [section group] (vertical, gap 8, padding 8)   ← N × .section
   └─ Nav Collapse icon (24×24)         ← collapse toggle, FA glyph `angle-left`
```

Sections are grouped into separator-divided clusters. The group frames are layout-only (no visible header). For the **User** console the clusters are 7 / 7 / 1 sections; the **Admin** console swaps in its own section set (Admin Management, Customers, Services, Reporting, Business Operations, Products, AI, Creative).

Width math: Panel has 0 horizontal padding; each section group adds 8 left + 8 right, so a 200px nav yields 184px-wide rows (200 − 16).

### 5.3 Console switching

`Console=User` and `Console=Admin` render different section sets in the same shell. The Switcher header is the entry point for changing context; the cluster contents change with `Console`.

### 5.4 Navigation content map (default instance data)

> **Platform-embedded only.** This map is the Unifonic Platform's own navigation structure. Apply it only when building *inside* the platform. For a **standalone** tool, ignore this map and populate the nav with the tool's own sections/items (or omit the nav entirely). See `Layout.md` → *Build Context*.

The complete parent → child structure as authored in the source file. **Displayed label** is what renders in the UI; **`Section` variant** is the Figma key (they differ in several cases — use the displayed label for build output). Sections with no children are single-link items (no chevron, no accordion).

**User console** — three separator-divided groups: **Products**, **Settings**, **Developers**. (In Figma the middle group frame is named `Support`; the intended display grouping is "Settings".)

| Group | Displayed label | `Section` variant | Children |
|---|---|---|---|
| Products | Flow Studio | Flow Studio | Dashboard, My flows, Templates, Excecutions* |
| Products | Campaigns | Multichannel Campaigns | — (single link) |
| Products | Chatbot | Chatbot | — (single link) |
| Products | Integrations | Integrations | — (single link) |
| Products | Authenticate | Authenticate | My Applications, Reports |
| Products | uLink | uLink | Single URL, Bulk URL |
| Products | Notice | Notice | — (single link) |
| Settings | AI Studio | AI | Knowledge Base, Personas, Brand Voice |
| Settings | Reports & Logs | Reports&Logs | Dashboard, Log Analyzer, Reports, Message Logs, Voice Call logs, Scheduled Reports, Push notification reports, Push notification Logs |
| Settings | Admin | Admin | Users, Sub-Accounts, Security, Requests |
| Settings | Audiences | Audiences | Dashboard, Contacts, Segments, Data |
| Settings | Channels | Channels | SMS, Voice, WhatsApp, Facebook, Web Widget, Push Notifications |
| Settings | Content Hub | Creative | — (single link) |
| Settings | Library | Library | Audio, Catalog |
| Developers | Developers | Developers | Applications, Documentation, Webhook management |

Order within the nav is top-to-bottom as listed; a `uni-separator` sits between Products/Settings and between Settings/Developers. The Switcher header sits above Products; the collapse toggle sits below Developers.

**Admin console** — four separator-divided groups: **Customers**, **Products**, **Business Ops**, **Settings**. There is **no Switcher header** on the Admin console (it starts straight into the first group). (In Figma the group frames are named `Section`/`Section`/`Support`/`Support` and don't match these intended labels; rename in source to align.)

| Group | Displayed label | `Section` variant | Children |
|---|---|---|---|
| Customers | Customers | Customers | Users, Accounts, Pending Requests, Payment history, Balance, Unit, Requests history |
| Products | Products | Products | Campaigns, Flow Studio, uLink, Authenticate, Notice, Integrations, Audio library |
| Products | Services | Services | Sender IDs, Caller IDs, Voice applications, WhatsApp, Applications, Caller number, Facebook applications |
| Products | Reporting | Reporting | Dashboards, Log analyzer, Reports, Voice call logs, Masked call logs |
| Business Ops | Business Ops | Business Operations | Countries, Operators, Providers, Routing rules, Custom routing rules, Routing Checker, Operator Report |
| Settings | Admin | Admin Management | Users, Impersonation Activity, Admin logs |

Order is top-to-bottom as listed; a `uni-separator` sits between each group. The collapse toggle sits below the Settings group. ("Admin" / Admin Management carries a trailing-space label in source.)

\* `Excecutions` is a typo in the source file (should be `Executions`). Reproduced verbatim here; correct in Figma and the build will follow.

Notes: the leading icon for each parent is the matching SVG in the icon set (see [`./menu-icons/MANIFEST.md`](./menu-icons/MANIFEST.md); one SVG per `Section`, shared across collapsed/expanded). Child rows carry no leading icon (empty 24px slot per §4.3) and no chevron. The two consoles' `Reports & Logs` (User) and `Reporting` (Admin) share the same logo.

---

## 6. Implementation notes (read before generating)

1. **Reserve the leading slot.** The 24px leading icon box is always present, even for child rows with no logo. Alignment depends on it. (§4.3)
2. **Two icon systems.** Leading logos = inline SVG (instance-swap). Trailing chevron = FontAwesome glyph as text. Don't unify them. (§3.4)
3. **Cross-ramp token binding is intentional.** Label text uses `color/icon/boldest` in Pressed/Expanded; the chevron stays `color/icon/subtle` everywhere. Keep these as-is. (§3.3)
4. **Chevron direction encodes open state.** `angle-down` = closed, `angle-up` = open (Expanded/Selected). (§3.3)
5. **Use raw Light-mode hex as build values.** This build does not consume the token structure; the token names in §2/§3.3 only identify a color's purpose. Pull the Light hex. If theming Dark, use the Dark column in §2.

---

## 7. The word "Collapsed" means two different things

This is the single most error-prone part of the system. The prop name `Collapsed` appears at two levels with **different semantics**:

| Level | `Collapsed=True` means |
|---|---|
| `uni-menu` (`State=Collapsed`) | **Rail mode** — whole nav shrinks to 40px, every row becomes a 24px icon-only button, labels and chevrons hidden. |
| `.uni-menu-item` (`Collapsed=True`) | **Rail mode** for that single row (24×24 icon-only). Set by the parent nav state. |
| `.section` (`Collapsed=True`) | **Accordion closed** — only the parent row shows; child rows are hidden. Nothing to do with rail width. |

When generating code, model these as two separate concepts:
- **railCollapsed** (nav-level): expanded 200px ↔ icon rail 40px.
- **sectionOpen** (section-level): accordion open ↔ closed.

A section can be open or closed independently of whether the nav is in rail mode.

---

## 8. Runtime behaviors NOT encoded in the component set

> [!WARNING]
> **NOT-YET-CONFIRMED — do not build from this section without verification.**
>
> Everything in §1–§7 is verified spec, authored from the Figma component set. The three behaviors below are **not** that. They are observed only in design screenshots, are **not** captured as Figma component variants, and have **not** been confirmed against the live product. Treat them as design intent to validate — not as buildable spec.
>
> If you are generating UI: **do not implement these from this file.** Either confirm the behavior against the live product first, or surface it as an open question and leave it out. Do not infer the missing details.

| # | Behavior | What's unconfirmed |
|---|---|---|
| 1 | **Rail flyout** | In rail mode (40px), hovering/clicking a section icon is believed to open a flyout panel showing that section's label + children to the right of the rail. Trigger (hover vs click), timing, and panel anatomy are all unverified. |
| 2 | **Mobile overlay** | On narrow viewports the nav and its sub-menus may render as an overlay/dropdown sheet (a "Select Menu Item" popover anchored to the trigger) rather than an inline accordion. The breakpoint and the overlay's structure are unverified. |
| 3 | **Collapse toggle direction** | The `Nav Collapse icon` shows `angle-left` when expanded (collapse). It is *expected* to show `angle-right` in rail mode (expand), but the rail variant's glyph is unconfirmed. |
