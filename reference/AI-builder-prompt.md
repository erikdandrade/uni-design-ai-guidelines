# UNI Design System — Project Reference

This project uses the **UNI Design System**. Three reference files are uploaded alongside this prompt:

- **`Design-guidelines.md`** — Typography, layouts, spacing, radius, elevation, colors (usage summary), and component specs.
- **`Color-tokens.md`** — Full color token reference with hex values per role (text, background, surface, border, divider, icon).
- **`Layout.md`** — Shell structure and layout modes (Default with Expanded/Collapsed nav states; Focus Mode with Full Width and Split View variants).

Treat these as the authoritative source for every design decision in this project. This is a **lightweight prototype** — apply the documented values (hex codes, type sizes, spacings, radii, shadows) directly in code. Don't build token infrastructure; visual fidelity is the goal. If Lovable generates shadcn or other component defaults, override their colors, typography, and radii to match the UNI tokens.

## Rules

1. **Documented values only.** Every color, font size, font weight, line-height, spacing, border radius, and shadow must come from these reference files. When you apply one, cite the token name and value in a comment or commit message (e.g. *"`color/text/primary` `#171717`"*) so the design intent stays traceable.

2. **Flag gaps — don't invent.** If a needed style is missing (a hex marked `TBD`, an undocumented font size, an unspecified component variant), stop and surface the gap rather than filling it with a guess.

3. **Typography — heading vs body at the same size.** When two styles share the same font size (14px or 16px), use the **heading** variant for single-line UI elements (tabs, labels, compact buttons) and the **body** variant for prose, inputs, paragraphs, and breathable form sub-sections. See *Choosing Between Heading and Body at the Same Size* in `Design-guidelines.md`.

4. **Layouts.** Every screen must be built on one of the documented modes:
   - **Default** — navigation-dependent views (tables, lists, dashboards, overviews). Side Nav visible, Expanded (200px) or Collapsed (48px).
   - **Focus Mode → Full Width** — wizards, multi-step flows, dense single-surface configurations. Top Bar visible, no Side Nav.
   - **Focus Mode → Split View** — creation flows with a setup pane on the left and a live preview pane on the right. Top Bar replaced by a Local Header.
   - Focus Mode is always entered from a Default view and must include a clear exit path back (close action in the local header).

5. **Components.** You may build any UI component (cards, tables, modals, forms, charts, navigation, etc.) as long as it is styled using the documented tokens. The Button specification in `Design-guidelines.md` is the level-of-detail model when introducing a new component pattern (anatomy → sizing → hierarchy → interaction states).

6. **Accessibility.** Maintain the contrast ratios stated in `Design-guidelines.md`: 4.5:1 for standard text, 3:1 for large typography and graphic UI indicators.

## When generating

Anchor every visual choice in a specific section of one of the reference files. If a choice can't be anchored, that's a gap — flag it before proceeding.
