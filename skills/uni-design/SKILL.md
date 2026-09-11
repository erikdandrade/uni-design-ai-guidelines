---
name: uni-design
description: Apply the UNI Design System when building, editing, or reviewing UI for a Unifonic product — screens, components, layouts, navigation, or icons. Trigger on requests like "build a screen", "generate this UI", "design this page/component", "implement the side nav/top bar", or any task that produces visual Unifonic UI.
---

# UNI Design System

Before generating or styling any UI, read `UNI-Design-AI-Guidelines/START-HERE.md` in full, then `AI-builder-prompt.md` for the complete rule set — documented-values-only, gap-flagging, layout modes, enumerated component states, accessibility. Between them they name which companion file covers which area:

- `Design-guidelines.md` — typography, spacing, radius, elevation, colors, components
- `Color-tokens.md` — full color-token → hex reference
- `Layout.md` — shell structure and layout modes
- `Side-navigation.md` — the `uni-menu` side nav: anatomy, states, content map
- `Top-bar.md` — the Main Top Bar / Local Header swap pair

**Run the question gate before generating anything.** It is three questions, asked in order, each skipped only on the stated condition — do not collapse them into one:

1. **Build context** — inside the Unifonic Platform, or a standalone tool? Standalone skips the platform chrome entirely and skips questions 2 and 3.
2. **Layout mode → header** *(platform-embedded only)* — Default, Focus Mode → Full Width, or Focus Mode → Split View? Decides Main Top Bar vs. Local Header, and whether the side nav renders at all. This is its own gate; it is not implied by question 1.
3. **Side-nav icons** *(platform-embedded + Default only)* — which Console (User/Admin) and Section(s), so the correct section logo is pulled from `menu-icons/` per `MANIFEST.md`.

Skip a question only when the request already answers it.

**Icons**: the 20 section-logo SVGs live in `UNI-Design-AI-Guidelines/menu-icons/` (mapped in `MANIFEST.md`). Import the local `.svg` file, or inline the raw `<svg>…</svg>` markup when the output must be self-contained. **Never reference an icon by external URL** — Claude.ai Artifacts' CSP blocks external requests outright.

Anchor every visual choice to a specific line in one of these files. If a value can't be anchored, that's a gap — flag it, don't invent it.
