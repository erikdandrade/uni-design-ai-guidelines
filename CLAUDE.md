# CLAUDE.md — Project orientation & session log

**Read this file first each session.** It is the map of this repo so you don't have to re-read every document. Drill into a specific guideline file only when the task needs its detail. When you change something meaningful, append a dated entry to the **Session Log** at the bottom.

---

## What this project is

This repo is **not an application** — it is the **UNI Design System reference set** ("UNI Design AI Guidelines"): a curated set of Markdown specs + icon assets used to steer **AI-assisted UI generation** (primarily Lovable). The specs are authored from the live Figma *UNI Design System* file and written for LLM consumption. The goal is visual fidelity in lightweight prototypes, not token infrastructure — so specs give **raw Light-mode hex build values**, naming tokens only to identify intent.

## Repository map

```
UNI-Design-AI-Guidelines/          ← the authoritative specs (formerly reference/)
├── AI-builder-prompt.md            ← the prompt injected into Lovable: authority + anti-invention rules
├── Design-guidelines.md            ← typography, spacing, radius, elevation, colors, components (Containers, Button, Side Nav, Platform Headers)
├── Color-tokens.md                 ← full color-token → hex reference
├── Layout.md                       ← shell structure + layout modes (Default, Focus Mode: Full Width / Split View)
├── Side-navigation.md              ← uni-menu spec: anatomy, states, Console→Section→child content map
├── Top-bar.md                      ← uni-top-bar (Main Top Bar) + uni-local-header (Local Header): anatomy, visibility matrix
└── menu-icons/                     ← 20 section-logo SVGs + MANIFEST.md (Section → file mapping)

external/                           ← untracked scratch/source (uni-menu.md, uni-menu-icons/); not part of the guidelines
```

## Key facts (don't re-derive these)

- **Relative links.** All cross-file links inside the guidelines use `./file.md` form — folder-agnostic, survive folder renames. There is no hardcoded folder path in any doc.
- **Header swap pair.** The shell's top row renders exactly one header: **Main Top Bar** in Default mode, **Local Header** throughout Focus Mode (both Full Width *and* Split View). Embedded apps (Agent Console, Chatbot) render neither. Both are **72px**. Full spec in `Top-bar.md`.
- **Anti-invention rules** live in `AI-builder-prompt.md`: documented values only; a geometry-only region is a *gap* (render empty + flag, don't populate); enumerated states are *closed* (add no background/border/shadow not listed); strip shadcn structural defaults not in the spec. These exist because Lovable once invented a whole top bar and a green selected-row pill.
- **Icon manifest** (`menu-icons/MANIFEST.md`) is verified consistent with the Side-nav content map: 20 unique SVGs, `reports-and-logs-logo.svg` shared by User→`Reports&Logs` and Admin→`Reporting`.
- **How the guidelines reach Lovable:** recommended path is **GitHub two-way sync** (full specs) + **Knowledge base** entry (the `AI-builder-prompt.md` rules + a "read the relevant `/UNI-Design-AI-Guidelines/*.md` file" pointer; Knowledge caps at ~10,000 chars, so the bulk specs stay in the synced repo).

## How to work in this repo

- Treat the files under `UNI-Design-AI-Guidelines/` as the single source of truth. When a value can't be anchored to a doc, that's a gap — flag it, don't guess.
- **`.pen` files** (if any appear) must be read/written only via the `pencil` MCP tools, never `Read`/`Grep`.
- On `main`: branch before committing unless told otherwise. Commit/push only when asked.
- After any meaningful change, add a Session Log entry below (reverse-chronological, newest first).

---

## Session Log

_Append newest entries at the top. Keep each entry to what changed and why — commit history holds the line-level detail._

### 2026-07-06
- **Diagnosed** why Lovable mis-generated the Top Bar and Side Navigation: the Top Bar had no anatomy spec (only geometry), so the model invented its contents (green "U" logo, global search, icon cluster, avatar); the side nav's Selected state wasn't declared closed, so a shadcn green selected-row pill survived.
- **Added `Top-bar.md`** documenting both platform headers from Figma — `uni-top-bar` (node `47959:147152`) and `uni-local-header` (node `54172:3214`): anatomy, prop-toggled sections, tokens, and a visibility matrix.
- **Reconciled** the header story across docs: `Layout.md` (Local Header height **65→72px**; Focus Mode swaps Main Top Bar → Local Header in *both* Full Width and Split View; relabeled layout-mode tables); `Design-guidelines.md` (added §7 *Platform Headers*); `AI-builder-prompt.md` (hardened anti-invention rules — geometry-gap clause, enumerated-states-closed clause, shadcn structural-default override; fixed Full Width header wording).
- **Verified** `menu-icons/MANIFEST.md` matches the content map (20 icons, shared `reports-and-logs-logo.svg`).
- **Renamed** `reference/` → `UNI-Design-AI-Guidelines/` and updated the prose in `AI-builder-prompt.md` (title → "AI Guidelines"; "reference files" → "UNI Design AI Guidelines"). All relative links unaffected.
- **Added this `CLAUDE.md`** as the per-session orientation + log doc.
- **Migrated the repo to the company Enterprise Cloud org.** `origin` now = `emanrique_unf/uni-design-ai-guidelines` (pushed); the original personal repo (`erikdandrade/uni-design-guidelines`) is kept as the `personal` remote backup.
- _Status: committed to `main`; `origin` is the company repo. `external/` left untracked as before._

### Before 2026-07-06
- See `git log` for prior history (initial commit, AI builder prompt, container family / platform surface background, Split View uni-form inlining rule).
