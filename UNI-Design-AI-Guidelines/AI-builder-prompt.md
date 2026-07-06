# UNI Design System — AI Guidelines

This project uses the **UNI Design System**. The following files — the **UNI Design AI Guidelines** — are uploaded alongside this prompt:

- **`Design-guidelines.md`** — Typography, layouts, spacing, radius, elevation, colors (usage summary), and component specs.
- **`Color-tokens.md`** — Full color token reference with hex values per role (text, background, surface, border, divider, icon).
- **`Layout.md`** — Shell structure and layout modes (Default with Expanded/Collapsed nav states; Focus Mode with Full Width and Split View variants).
- **`Side-navigation.md`** — The `uni-menu` side navigation component: anatomy, states, the full Console → Section → child content map, and rail/accordion behavior. Section logos are in `menu-icons/`.
- **`Top-bar.md`** — The two platform headers: the **Main Top Bar** (`uni-top-bar`) and the **Local Header** (`uni-local-header`). Anatomy, prop-toggled sections, tokens, and the visibility matrix (which surface renders in which layout mode).

Treat these as the authoritative source for every design decision in this project. This is a **lightweight prototype** — apply the documented values (hex codes, type sizes, spacings, radii, shadows) directly in code. Don't build token infrastructure; visual fidelity is the goal. If Lovable generates shadcn or other component defaults, override not just their colors, typography, and radii but also their **structural defaults** — selected/hover row backgrounds, focus rings, dividers, and any furniture the generator injects on its own (search fields, avatar tiles, icon clusters) — to match the UNI specs. A default that isn't in the spec must be removed, not restyled.

## Rules

1. **Documented values only.** Every color, font size, font weight, line-height, spacing, border radius, and shadow must come from the **UNI Design AI Guidelines**. When you apply one, cite the token name and value in a comment or commit message (e.g. *"`color/text/primary` `#171717`"*) so the design intent stays traceable.

2. **Flag gaps — don't invent.** If a needed style is missing (a hex marked `TBD`, an undocumented font size, an unspecified component variant), stop and surface the gap rather than filling it with a guess. **A region specified only by geometry counts as a gap, not as freedom.** If a surface has a defined size/placement but no documented anatomy (its contents, slots, and elements), do **not** populate it with conventional elements (logos, global search, avatars, icon trays). Render it empty or as a labeled placeholder and flag it. Absence of an anatomy spec is a signal to ask, never a license to fill.

3. **Typography — heading vs body at the same size.** When two styles share the same font size (14px or 16px), use the **heading** variant for single-line UI elements (tabs, labels, compact buttons) and the **body** variant for prose, inputs, paragraphs, and breathable form sub-sections. See *Choosing Between Heading and Body at the Same Size* in `Design-guidelines.md`.

4. **Layouts.** Every screen must be built on one of the documented modes:
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
