---
name: uni-design
description: Apply the UNI Design System when building, editing, or reviewing UI for a Unifonic product — screens, components, layouts, navigation, or icons. Trigger on requests like "build a screen", "generate this UI", "design this page/component", "implement the side nav/top bar", or any task that produces visual Unifonic UI.
---

# UNI Design System

Before generating or styling any UI, read `UNI-Design-AI-Guidelines/AI-builder-prompt.md` in full. It is the authoritative rule set — documented-values-only, gap-flagging, layout modes, enumerated component states, accessibility — and it names which companion file covers which area:

- `Design-guidelines.md` — typography, spacing, radius, elevation, colors, components
- `Color-tokens.md` — full color-token → hex reference
- `Layout.md` — shell structure and layout modes
- `Side-navigation.md` — the `uni-menu` side nav: anatomy, states, content map
- `Top-bar.md` — the Main Top Bar / Local Header swap pair

Work through the **"Ask this first"** gate in `AI-builder-prompt.md` before generating anything, in order, skipping only what the request already makes clear: (1) build context — "inside the Unifonic Platform, or standalone tool?"; (2) if platform-embedded, layout mode — Default / Focus Mode → Full Width / Focus Mode → Split View, which decides the header; (3) if platform-embedded *and* Default, which Console/Section for the side-nav icon.

**Icons**: the 20 section-logo SVGs live in `UNI-Design-AI-Guidelines/menu-icons/` (mapped in `MANIFEST.md`). Inline the raw `<svg>…</svg>` markup directly into generated code — never reference an icon by external URL.

Anchor every visual choice to a specific line in one of these files. If a value can't be anchored, that's a gap — flag it, don't invent it.
