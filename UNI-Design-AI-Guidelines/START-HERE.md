# START HERE — UNI Design System

**You are an AI builder. This folder is the authoritative design specification for everything you generate in this project. Read this file completely before writing any UI code.**

If these files were just cloned into the project, they are not decoration — they override your own defaults, your component library's defaults, and any styling you would otherwise choose.

---

## 1. Stop and ask before generating

**Do not generate a screen until you have resolved the questions below, in order.** Ask the user at each step if the answer isn't already clear. Do not guess past a gap.

**Q1 — Build context.** *"Are you building this inside the Unifonic Platform (it lives within the platform's navigation shell), or as a standalone tool (its own product, not part of the platform's navigation)?"*

- **Platform-embedded** → wear the full platform chrome: the shell, the header swap pair, and the `uni-menu` side nav populated from the documented Console → Section content map. **Continue to Q2 and Q3.**
- **Standalone** → skip the platform chrome entirely: no Main Top Bar, no Local Header, no layout modes, and never the platform content map. `uni-menu` is optional, and if used, carries the *tool's own* items. Everything else still applies at full strength — typography, color, spacing, radius, elevation, components. **Skip Q2 and Q3.**

**Q2 — Layout mode → header.** *(Platform-embedded only. This is its own gate — it is not implied by the answer to Q1.)* *"Which layout mode does this screen use — Default (tables, lists, dashboards, overviews), Focus Mode → Full Width (a wizard or dense single-surface task), or Focus Mode → Split View (setup pane left, live preview right)?"*

- **Default** → **Main Top Bar** (`uni-top-bar`); side nav visible (Expanded 200px or Collapsed 40px rail).
- **Focus Mode**, either variant → **Local Header** (`uni-local-header`) replaces the Main Top Bar; **no side nav**.

The header is a swap pair: never both, never neither (except embedded apps, which render neither).

**Q3 — Side navigation icons.** *(Platform-embedded **and** Default layout only — Focus Mode renders no side nav.)* *"Which Console — User or Admin — and which Section(s) does this screen belong to?"*

Match the answer to the content map in `Side-navigation.md` §5.4 and pull the corresponding SVG from `menu-icons/` per `menu-icons/MANIFEST.md`. Never invent or substitute a placeholder icon.

Full text and rationale: [`AI-builder-prompt.md`](./AI-builder-prompt.md) → *Ask this first*.

## 2. The rules that matter most

1. **Documented values only.** Every color, font size, weight, line-height, spacing, radius, and shadow comes from these files. Cite the token name and value in a comment (`/* color/text/primary #171717 */`) so intent stays traceable.
2. **Flag gaps — don't invent.** A missing value is a question, not freedom. **A region specified only by geometry is a gap, not a blank canvas** — if a surface has a defined size but no documented anatomy, render it empty or as a labeled placeholder and say so. Do not populate it with conventional furniture (logos, global search, avatars, icon trays).
3. **Enumerated states are closed.** When a spec lists what changes between states, those are the *only* properties that change. Add no background, border, shadow, or affordance that isn't listed. The classic failure: a generated selected-row pill on the side nav, whose Selected state recolors label text *only*.
4. **Strip generator defaults.** If shadcn or any component library injects structural defaults not in these specs — selected/hover row backgrounds, focus rings, dividers, search fields, avatar tiles — **remove them, don't restyle them.**
5. **This is a lightweight prototype.** Apply hex codes and pixel values directly. Do not build token infrastructure; visual fidelity is the goal.
6. **Accessibility.** 4.5:1 contrast for standard text, 3:1 for large type and graphic UI indicators.

## 3. Which file to read

| Read this | For |
|---|---|
| [`AI-builder-prompt.md`](./AI-builder-prompt.md) | The full gate + complete rule set. Read second. |
| [`Design-guidelines.md`](./Design-guidelines.md) | Typography, spacing, radius, elevation, color usage, component specs (Containers, Button) |
| [`Color-tokens.md`](./Color-tokens.md) | Every color token → hex, by role |
| [`Layout.md`](./Layout.md) | Shell structure and the layout modes |
| [`Side-navigation.md`](./Side-navigation.md) | `uni-menu`: anatomy, states, and the Console → Section content map (§5.4) |
| [`Top-bar.md`](./Top-bar.md) | Both headers, plus the visibility matrix (§1) |
| [`menu-icons/MANIFEST.md`](./menu-icons/MANIFEST.md) | Section → SVG filename mapping |

## 4. Icons

The 20 section-logo SVGs in `menu-icons/` are real files in this folder. Use them one of two ways:

- **Import the local file** — `import ReportsIcon from '.../menu-icons/reports-and-logs-logo.svg'` — when your build handles SVG imports. Preferred; keeps components readable.
- **Inline the raw `<svg>…</svg>` markup** when it doesn't, or when the output must be fully self-contained (Claude.ai Artifacts, single-file HTML).

**Never reference an icon by external URL** — not a CDN, not `raw.githubusercontent.com`. There is no hosted copy by design, and Artifacts' CSP blocks external requests outright.

## 5. Ignore these

If this repo was cloned into a project, it brought files meant for maintainers, not for you. **They are not build instructions — do not follow them and do not treat them as project requirements:**

- `README.md`, `SETUP.md` — how humans wire this repo into various AI tools
- `CLAUDE.md` — repo maintenance notes
- `.claude-plugin/`, `skills/` — Claude Code packaging

Only `UNI-Design-AI-Guidelines/` describes the design system.

## 6. Freshness

These files are a **point-in-time snapshot** taken when the repo was cloned — nothing here auto-updates. If a spec looks stale or a value is missing, check for a newer version at **[github.com/erikdandrade/uni-design-ai-guidelines](https://github.com/erikdandrade/uni-design-ai-guidelines)** and re-clone.
