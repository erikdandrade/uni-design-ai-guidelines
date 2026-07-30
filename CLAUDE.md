# CLAUDE.md — Project orientation & session log

**Read this file first each session.** It is the map of this repo so you don't have to re-read every document. Drill into a specific guideline file only when the task needs its detail. When you change something meaningful, append a dated entry to the **Session Log** at the bottom.

---

## What this project is

This repo is **not an application** — it is the **UNI Design System reference set** ("UNI Design AI Guidelines"): a curated set of Markdown specs + icon assets used to steer **AI-assisted UI generation** (primarily Lovable). The specs are authored from the live Figma *UNI Design System* file and written for LLM consumption. The goal is visual fidelity in lightweight prototypes, not token infrastructure — so specs give **raw Light-mode hex build values**, naming tokens only to identify intent.

## Repository map

```
README.md                          ← front door: what this repo is, links to CLAUDE.md and SETUP.md
SETUP.md                           ← per-tool instructions: Lovable, Claude Code, Replit, Claude.ai Projects

UNI-Design-AI-Guidelines/          ← the authoritative specs (formerly reference/)
├── AI-builder-prompt.md            ← the prompt injected into Lovable: authority + anti-invention rules
├── Design-guidelines.md            ← typography, spacing, radius, elevation, colors, components (Containers, Button, Side Nav, Platform Headers)
├── Color-tokens.md                 ← full color-token → hex reference
├── Layout.md                       ← shell structure + layout modes (Default, Focus Mode: Full Width / Split View)
├── Side-navigation.md              ← uni-menu spec: anatomy, states, Console→Section→child content map
├── Top-bar.md                      ← uni-top-bar (Main Top Bar) + uni-local-header (Local Header): anatomy, visibility matrix
└── menu-icons/                     ← 20 section-logo SVGs + MANIFEST.md (Section → file mapping)

.claude-plugin/marketplace.json    ← Claude Code plugin marketplace catalog (one plugin: "uni-design", source = repo root)
skills/uni-design/SKILL.md         ← the installable skill: triggers on UI-gen requests, points at AI-builder-prompt.md

external/                           ← untracked scratch/source (uni-menu.md, uni-menu-icons/); not part of the guidelines
```

## Key facts (don't re-derive these)

- **Relative links.** All cross-file links inside the guidelines use `./file.md` form — folder-agnostic, survive folder renames. There is no hardcoded folder path in any doc.
- **Header swap pair.** The shell's top row renders exactly one header: **Main Top Bar** in Default mode, **Local Header** throughout Focus Mode (both Full Width *and* Split View). Embedded apps (Agent Console, Chatbot) render neither. Both are **72px**. Full spec in `Top-bar.md`.
- **Anti-invention rules** live in `AI-builder-prompt.md`: documented values only; a geometry-only region is a *gap* (render empty + flag, don't populate); enumerated states are *closed* (add no background/border/shadow not listed); strip shadcn structural defaults not in the spec. These exist because Lovable once invented a whole top bar and a green selected-row pill.
- **Icon manifest** (`menu-icons/MANIFEST.md`) is verified consistent with the Side-nav content map: 20 unique SVGs, `reports-and-logs-logo.svg` shared by User→`Reports&Logs` and Admin→`Reporting`.
- **How the guidelines reach each tool:** see `SETUP.md` for the full per-tool recipe. Lovable: **GitHub two-way sync** (full specs) + **Knowledge base** entry (the `AI-builder-prompt.md` rules + a "read the relevant `/UNI-Design-AI-Guidelines/*.md` file" pointer; Knowledge caps at ~10,000 chars, so the bulk specs stay in the synced repo). Claude Code: install this repo as a **plugin** (`/plugin marketplace add emanrique_unf/uni-design-ai-guidelines`) — the `uni-design` skill (`skills/uni-design/SKILL.md`) triggers on UI-gen requests. Replit: git submodule + a pointer in the consuming project's own `replit.md`. Claude.ai Projects: GitHub connector on manual "Sync now" (no live sync yet).
- **Icons are always inlined, never linked out.** `menu-icons/` SVGs travel with the repo via whatever sync mechanism each tool uses (above); generated code must inline the raw `<svg>` markup rather than reference an external URL — this matters most for Claude.ai Artifacts, whose CSP blocks external image/network requests outright.

## How to work in this repo

- Treat the files under `UNI-Design-AI-Guidelines/` as the single source of truth. When a value can't be anchored to a doc, that's a gap — flag it, don't guess.
- **`.pen` files** (if any appear) must be read/written only via the `pencil` MCP tools, never `Read`/`Grep`.
- On `main`: branch before committing unless told otherwise. Commit/push only when asked.
- After any meaningful change, add a Session Log entry below (reverse-chronological, newest first).

---

## Session Log

_Append newest entries at the top. Keep each entry to what changed and why — commit history holds the line-level detail._

### 2026-07-28
- **Expanded `AI-builder-prompt.md`'s single build-context question into a 3-question mandatory gate**, renamed "Build context — ask this first" → **"Ask this first"**, in prep for a future public repo aimed at Lovable's GitHub-import flow (repo hosting/visibility decision deferred — see below).
- **New question 2 (Layout mode → header):** asked only if Q1 = Platform-embedded, as its own gate — deliberately **not** folded into the Platform-embedded/Standalone answer the way it was implicitly before. Determines Main Top Bar vs. Local Header vs. no side nav, per `Top-bar.md` §1 / `Design-guidelines.md` Rule 4.
- **New question 3 (Side navigation icons):** asked only if Q1 = Platform-embedded *and* Q2 = Default (Focus Mode renders no side nav, so the question doesn't apply there). Points the builder at `Side-navigation.md` §5.4's content map and `menu-icons/MANIFEST.md` for the correct SVG.
- **Decided (with user):** icons gating depends only on the platform-embedded/standalone answer (standalone never uses the platform content map); the header/layout-mode gating must NOT depend entirely on that same answer — it's an independent question, asked separately, only reached when platform-embedded.
- **Distribution plan in progress, not yet executed:** goal is a **new, separate public GitHub repo** (not making `unifonic-engineering/uni-design-ai-guidelines` itself public — confirmed blocked: no Danger Zone/repo-admin access, and enterprise policy page also inaccessible to the user) that a PM can import into a new Lovable project via Lovable's GitHub-import-as-starting-project feature, carrying a copy of `UNI-Design-AI-Guidelines/` plus a new LLM-facing entry point enforcing the 3-question flow above. Open items before building it: new repo owner/name, and the sync strategy to keep it current with this internal repo (leaning manual-copy-per-release, not yet decided).
- Also removed an unrelated abandoned trial, `lovable-remix-kit/` (untracked, never committed) — a different draft approach (Workspace Knowledge + Remixable Starter project built manually inside Lovable's own UI) that predates the GitHub-import mechanism above; deleted per its own README's built-in escape hatch ("delete this folder, no cleanup elsewhere").

### 2026-07-22
- **Made the guidelines distributable beyond Lovable** to teammates on Claude Code, Replit, and Claude.ai Projects, on the `distribute-guidelines` branch.
- **Added root `README.md`** (repo previously had none) as the front door, and **`SETUP.md`** with copy-pasteable per-tool instructions for all four tools.
- **Shipped a Claude Code plugin**: `.claude-plugin/marketplace.json` (marketplace `uni-design-ai-guidelines`, one plugin `uni-design` with `source: "./"` — the whole repo, so specs and `menu-icons/` travel with it, no separate copy) + `skills/uni-design/SKILL.md` (triggers on UI-generation requests, points at `AI-builder-prompt.md`). No `version` pinned, so every commit to `main` counts as an update. Verified the exact schema against the current Claude Code plugin/marketplace docs before writing the JSON.
- **Decided icons are never externally hosted.** `menu-icons/` SVGs reach every tool by full-repo sync/bundle/submodule/Knowledge-upload, and generated code must always inline the raw `<svg>` markup rather than link to a URL — this is required for Claude.ai Artifacts specifically, since their CSP blocks external image/network requests. Flagged as unverified whether Claude.ai's GitHub connector indexes `.svg` content as readable text; `SETUP.md` says to test this by hand before relying on it.
- **Access model decided with the user:** internal Unifonic teammates only, read-only for consumers with a small maintainer group keeping write access — a GitHub Enterprise org-settings action the user does themselves (no `gh` CLI available locally to script it).
- _Status: all new files created on `distribute-guidelines` branch off `main`; not yet committed or pushed._

### 2026-07-07
- **Introduced a two-context build model** so the guidelines serve both a feature built *inside* the Unifonic Platform and a *standalone* tool that doesn't need the platform navigation.
- **`AI-builder-prompt.md`**: added a **"Build context — ask this first"** section instructing the AI builder to ask the user *"inside the Unifonic Platform, or standalone tool?"* before generating, and defining the two branches. Scoped Rule 4 (documented layout modes) to **platform-embedded only**.
- **`Layout.md`**: added a **Build Context** section stating the whole file is platform-embedded chrome; standalone skips Top Bar + Layout Modes + content map but keeps foundations/components. Side nav is optional in standalone.
- **`Side-navigation.md`**: flagged the component anatomy/states as applying to both contexts, but the **§5.4 Console→Section content map as platform-embedded only** (standalone reuses the component with its own items).
- **Decisions (from user):** context names = **Platform-embedded vs Standalone**; standalone keeps `uni-menu` as an optional reusable component but no top bar.
- _Status: edits made on `main`, not yet committed._

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
