# UNI Design System — AI Guidelines

This project uses the **UNI Design System**. The following files — the **UNI Design AI Guidelines** — sit alongside this one, whether they were cloned into the project, synced as Knowledge, or bundled by a plugin:

- **`START-HERE.md`** — The short entry point: the mandatory question gate, the rules that matter most, and which file to read for what. Read it first if you haven't.
- **`Design-guidelines.md`** — Typography, layouts, spacing, radius, elevation, colors (usage summary), and component specs.
- **`Color-tokens.md`** — Full color token reference with hex values per role (text, background, surface, border, divider, icon).
- **`Layout.md`** — Shell structure and layout modes (Default with Expanded/Collapsed nav states; Focus Mode with Full Width and Split View variants).
- **`Side-navigation.md`** — The `uni-menu` side navigation component: anatomy, states, the full Console → Section → child content map, and rail/accordion behavior. Section logos are in `menu-icons/`.
- **`Top-bar.md`** — The two platform headers: the **Main Top Bar** (`uni-top-bar`) and the **Local Header** (`uni-local-header`). Anatomy, prop-toggled sections, tokens, and the visibility matrix (which surface renders in which layout mode).

Treat these as the authoritative source for every design decision in this project. This is a **lightweight prototype** — apply the documented values (hex codes, type sizes, spacings, radii, shadows) directly in code. Don't build token infrastructure; visual fidelity is the goal. If Lovable generates shadcn or other component defaults, override not just their colors, typography, and radii but also their **structural defaults** — selected/hover row backgrounds, focus rings, dividers, and any furniture the generator injects on its own (search fields, avatar tiles, icon clusters) — to match the UNI specs. A default that isn't in the spec must be removed, not restyled.

## Ask this first

Before generating anything, resolve the following **in order**. **Ask the user** at each step if the answer isn't already clear from the request — do not guess past a gap.

### 1. Build context

> *"Are you building this **inside the Unifonic Platform** (it will live within the platform's navigation shell), or as a **standalone tool** (its own product, not part of the platform's navigation)?"*

The answer decides whether the platform navigation applies:

- **Platform-embedded** — the feature lives inside the Unifonic Platform. Wear the **full platform chrome**: the shell, the Main Top Bar / Local Header swap pair, and the `uni-menu` side navigation populated with the **documented Console → Section → child content map** (`Side-navigation.md` §5.4). Every screen uses one of the documented layout modes. This is the default when a request clearly concerns platform features (Flow Studio, Campaigns, Admin, etc.). Continue to questions 2 and 3 below.

- **Standalone** — a tool that is **not** part of the platform's navigation. **Skip the platform chrome:** no Main Top Bar, no Local Header, no layout modes, and **do not** apply the documented Console/Section content map. The `uni-menu` side navigation is **optional** — reuse it as a component only if the tool needs its own navigation, populated with the **tool's own sections/items** (never the platform content map). Everything else still applies at full strength: typography, color tokens, spacing, radius, elevation, and all components (Button, Containers, etc.). A standalone tool should still look unmistakably like Unifonic. **Skip questions 2 and 3 — neither header nor the icon content map applies.**

When in doubt, ask — do not assume platform embedding. See `Layout.md` → *Build Context* for the structural detail.

### 2. Layout mode → header (platform-embedded only)

Ask only if question 1 answered **Platform-embedded**:

> *"Which layout mode does this screen use — **Default** (a navigation-dependent view: tables, lists, dashboards, overviews), **Focus Mode → Full Width** (a wizard or dense single-surface task), or **Focus Mode → Split View** (a setup pane on the left with a live preview on the right)?"*

This is a separate gate from question 1 — it is not implied by "platform-embedded," it must be asked on its own. The answer decides the header (a swap pair — never both, never neither except embedded apps) and whether the side nav renders at all:

- **Default** → **Main Top Bar** (`uni-top-bar`); `uni-menu` side nav visible (Expanded or Collapsed).
- **Focus Mode → Full Width** or **Focus Mode → Split View** → **Local Header** (`uni-local-header`) replaces the Main Top Bar; **no side nav**.

See `Top-bar.md` §1 (visibility matrix) and `Design-guidelines.md` Rule 4 for full detail.

### 3. Side navigation icons (platform-embedded + Default layout only)

Ask only if question 1 answered **Platform-embedded** *and* question 2 answered **Default** (Focus Mode renders no side nav, so this question does not apply there):

> *"Which Console — **User** or **Admin** — and which Section(s) does this screen belong to, so the correct section-logo icon(s) from `menu-icons/` populate the side nav?"*

Match the answer against the documented Console → Section content map (`Side-navigation.md` §5.4) and pull the corresponding SVG from `menu-icons/` per `menu-icons/MANIFEST.md` — never invent or substitute a placeholder icon.

## Rules

1. **Documented values only.** Every color, font size, font weight, line-height, spacing, border radius, and shadow must come from the **UNI Design AI Guidelines**. When you apply one, cite the token name and value in a comment or commit message (e.g. *"`color/text/primary` `#171717`"*) so the design intent stays traceable.

2. **Flag gaps — don't invent.** If a needed style is missing (a hex marked `TBD`, an undocumented font size, an unspecified component variant), stop and surface the gap rather than filling it with a guess. **A region specified only by geometry counts as a gap, not as freedom.** If a surface has a defined size/placement but no documented anatomy (its contents, slots, and elements), do **not** populate it with conventional elements (logos, global search, avatars, icon trays). Render it empty or as a labeled placeholder and flag it. Absence of an anatomy spec is a signal to ask, never a license to fill.

3. **Typography — heading vs body at the same size.** When two styles share the same font size (14px or 16px), use the **heading** variant for single-line UI elements (tabs, labels, compact buttons) and the **body** variant for prose, inputs, paragraphs, and breathable form sub-sections. See *Choosing Between Heading and Body at the Same Size* in `Design-guidelines.md`.

4. **Layouts.** *(Platform-embedded builds only — see Build Context. Standalone tools do not use these modes; they lay out on their own surface with the platform chrome skipped.)* Every platform-embedded screen must be built on one of the documented modes:
   - **Default** — navigation-dependent views (tables, lists, dashboards, overviews). **Main Top Bar** visible (build from `Top-bar.md`); Side Nav visible, Expanded (200px) or Collapsed (40px rail). Build the nav from `Side-navigation.md` — see its content map for the sections and child routes per console.
   - **Focus Mode → Full Width** — wizards, multi-step flows, dense single-surface configurations. **Local Header replaces the Main Top Bar**; no Side Nav.
   - **Focus Mode → Split View** — creation flows with a setup pane on the left and a live preview pane on the right. **Local Header replaces the Main Top Bar.**
   - The header is a swap pair: render the Main Top Bar in Default and the Local Header in Focus Mode — never both. Embedded apps (Agent Console, Chatbot) render neither. See `Top-bar.md` for the visibility matrix.
   - Focus Mode is always entered from a Default view and must include a clear exit path back (close action in the Local Header).

5. **Components.** You may build any UI component (cards, tables, modals, forms, charts, navigation, etc.) as long as it is styled using the documented tokens. The Button specification in `Design-guidelines.md` is the level-of-detail model when introducing a new component pattern (anatomy → sizing → hierarchy → interaction states).

   **Enumerated states are closed.** When a spec lists what changes between a component's states (e.g. the side nav's state → token table, or a button's per-state tones), those are the **only** properties that change. Do not add a background fill, border, shadow, or other affordance that isn't listed. A shadcn default selected/hover row background is the classic offender — the UNI side nav's Selected state recolors the label text only, with no background pill, so strip the generated fill.

6. **Accessibility.** Maintain the contrast ratios stated in `Design-guidelines.md`: 4.5:1 for standard text, 3:1 for large typography and graphic UI indicators.

## When generating

Anchor every visual choice in a specific section of one of the UNI Design AI Guidelines files. If a choice can't be anchored, that's a gap — flag it before proceeding.
