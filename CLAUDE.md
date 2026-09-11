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
├── START-HERE.md                   ← builder entry point: the mandatory question gate + file index. Read before generating.
├── AI-builder-prompt.md            ← authority + anti-invention rules + the 3-question gate in full
├── Design-guidelines.md            ← typography, spacing, radius, elevation, colors, components (Containers, Button, Side Nav, Platform Headers)
├── Color-tokens.md                 ← full color-token → hex reference
├── Layout.md                       ← shell structure + layout modes (Default, Focus Mode: Full Width / Split View)
├── Side-navigation.md              ← uni-menu spec: anatomy, states, Console→Section→child content map
├── Top-bar.md                      ← uni-top-bar (Main Top Bar) + uni-local-header (Local Header): anatomy, visibility matrix
└── menu-icons/                     ← 20 section-logo SVGs + MANIFEST.md (Section → file mapping)

.claude-plugin/marketplace.json    ← Claude Code plugin marketplace catalog (one plugin: "uni-design", source = repo root)
skills/uni-design/SKILL.md         ← the installable skill: triggers on UI-gen requests, points at START-HERE.md

external/                           ← untracked scratch/source (uni-menu.md, uni-menu-icons/); not part of the guidelines
```

## Key facts (don't re-derive these)

- **Relative links.** All cross-file links inside the guidelines use `./file.md` form — folder-agnostic, survive folder renames. There is no hardcoded folder path in any doc.
- **Header swap pair.** The shell's top row renders exactly one header: **Main Top Bar** in Default mode, **Local Header** throughout Focus Mode (both Full Width *and* Split View). Embedded apps (Agent Console, Chatbot) render neither. Both are **72px**. Full spec in `Top-bar.md`.
- **Anti-invention rules** live in `AI-builder-prompt.md`: documented values only; a geometry-only region is a *gap* (render empty + flag, don't populate); enumerated states are *closed* (add no background/border/shadow not listed); strip shadcn structural defaults not in the spec. These exist because Lovable once invented a whole top bar and a green selected-row pill.
- **Icon manifest** (`menu-icons/MANIFEST.md`) is verified consistent with the Side-nav content map: 20 unique SVGs, `reports-and-logs-logo.svg` shared by User→`Reports&Logs` and Admin→`Reporting`.
- **The 3-question gate is mandatory** and must fire *before* any generation: build context (platform-embedded vs standalone) → layout mode/header → side-nav icons. Questions 2 and 3 are separately gated, not implied by question 1. Full text in `AI-builder-prompt.md` → *Ask this first*; the short form is in `START-HERE.md`.
- **How the guidelines reach each tool:** see `SETUP.md` for the full per-tool recipe. **Lovable's GitHub integration is export-only** (it cannot import/sync an external repo) — so Lovable is asked to **clone this repo into a subfolder** of the project, plus a short Knowledge-base pointer to `START-HERE.md` so the rule fires on every message. Claude Code: install this repo as a **plugin** (`/plugin marketplace add erikdandrade/uni-design-ai-guidelines`) — the `uni-design` skill (`skills/uni-design/SKILL.md`) triggers on UI-gen requests and points at `START-HERE.md`. Replit: git submodule + a pointer in the consuming project's own `replit.md`. Claude.ai Projects: GitHub connector on manual "Sync now" (no live sync yet), custom instructions point at `START-HERE.md`.
- **Icons: import the local file when the repo's files are physically present in the project (Lovable/Replit), otherwise inline the raw `<svg>` markup — never reference an icon by external URL.** This matters most for Claude.ai Artifacts, whose CSP blocks external image/network requests outright.
- **A Lovable clone is a point-in-time snapshot.** Nothing auto-updates there — consumers re-clone to refresh. Keep `START-HERE.md` accurate, since it's the only always-on instruction a clone carries (it explicitly tells the AI to ignore `README.md`/`SETUP.md`/`CLAUDE.md` as maintainer-only files).

## How to work in this repo

- Treat the files under `UNI-Design-AI-Guidelines/` as the single source of truth. When a value can't be anchored to a doc, that's a gap — flag it, don't guess.
- **`.pen` files** (if any appear) must be read/written only via the `pencil` MCP tools, never `Read`/`Grep`.
- On `main`: branch before committing unless told otherwise. Commit/push only when asked.
- After any meaningful change, add a Session Log entry below (reverse-chronological, newest first).

---

## Session Log

_Append newest entries at the top. Keep each entry to what changed and why — commit history holds the line-level detail._

### 2026-09-11 (2)
- **Ported the builder-facing distribution workflow in from `erikdandrade/unifonic-design`**, a separate, more-evolved public repo discovered mid-session that already implemented the "new public repo for PM/Lovable import" plan from 2026-07-28 (built 2026-08-03, outside this working directory, never logged here). Rather than keep two diverging public repos, the user decided `uni-design-ai-guidelines` is the one and only repo going forward — so its improvements were merged in here instead of retiring in place:
  - **Added `UNI-Design-AI-Guidelines/START-HERE.md`** — the short, builder-facing entry point (question gate, core rules, file index, and an explicit "ignore README/SETUP/CLAUDE.md" instruction for the AI). This is now the file every tool's setup recipe points an AI builder at first; `AI-builder-prompt.md` remains the full rule set (read second) and gained a line referencing it.
  - **Corrected `SETUP.md`'s Lovable section** — it previously claimed a "GitHub two-way sync," which is wrong: Lovable's GitHub integration is **export-only** (it owns/mirrors a repo it generates; it cannot import or read an external one). Replaced with the verified workflow: ask Lovable to clone this repo into a project subfolder, add a ~40-word Knowledge pointer to `START-HERE.md`, and verify the gate actually fires before trusting it.
  - **Updated `skills/uni-design/SKILL.md`** to read `START-HERE.md` first, and to prefer importing the local `.svg` file over inlining when the repo's files are physically present in the project (previously said "always inline").
- **The PM-facing Lovable prompt** (clone repo → subfolder → read `START-HERE.md`) now points at `erikdandrade/uni-design-ai-guidelines` and is added to `README.md`'s Quick start section — this is what gets handed to PMs.
- _Status: committed and pushed to `origin` (`erikdandrade/uni-design-ai-guidelines`), `main`. The old `erikdandrade/unifonic-design` repo was left untouched (not deleted) but is no longer the intended distribution point._

### 2026-09-11
- **Migrated canonical hosting off Unifonic-managed GitHub entirely, to the personal account `erikdandrade`.** Trigger: the user is losing access to `emanrique_unf`, the Unifonic-provisioned GitHub identity that owned the `origin` remote — so the plan from 2026-07-28 (a new public repo for outside distribution) got pulled forward and repurposed as the sole canonical repo, replacing `unifonic-engineering/uni-design-ai-guidelines` rather than sitting alongside it.
- **Correction to the 2026-07-06 entry below:** it claimed `origin` became `emanrique_unf/uni-design-ai-guidelines` after the company migration. That was never true — `origin` was actually `unifonic-engineering/uni-design-ai-guidelines`; `emanrique_unf` only ever existed as a personal fork used to open PRs into it (see PR merge commits `7213151`, `b9fcc61`). The wrong slug had propagated into `README.md` and every per-tool recipe in `SETUP.md` — found and fixed in this session.
- **New repo:** `erikdandrade/uni-design-ai-guidelines` (public, created fresh — distinct from the older `erikdandrade/uni-design-guidelines` backup mentioned in the 2026-07-06 entry, which is untouched). `main` and `distribute-guidelines` were fast-forward-merged first (bringing in the 3-question gate commit) and only the consolidated `main` was pushed; the branch split isn't being carried forward now that this is a solo-maintained personal repo.
- **Remotes:** old `origin` (`unifonic-engineering/...`) renamed to `unifonic` for reference, not deleted. New `origin` = the `erikdandrade` repo. `personal` (old backup) and `bitbucket` remotes left as they were.
- **Updated everywhere the old slugs appeared:** `README.md` (repo`emanrique_unf` → `erikdandrade`; Access model paragraph rewritten — no more GitHub Enterprise org-settings/read-only-via-org-permissions story, now a plain public personal repo with maintainer-discretion PR review), `SETUP.md` (Lovable sync target, `/plugin marketplace add`, Replit submodule URL), this file's "Key facts" plugin-install line, and `.claude-plugin/marketplace.json` (dropped `owner.email`, which had been the user's Unifonic work address — decided against carrying a company email into a public personal-account repo).
- **Not yet done:** the distribution-plan open items from 2026-07-28 (new-repo owner/name, sync strategy) are now resolved by this migration rather than needing a separate repo — that plan is effectively superseded, not still pending.
- _Status: committed and pushed to the new `origin` (`erikdandrade/uni-design-ai-guidelines`), `main` branch only._

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
