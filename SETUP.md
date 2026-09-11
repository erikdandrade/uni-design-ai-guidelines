# Setup — connecting your AI tool to the UNI Design AI Guidelines

Pick your tool below. In every case the goal is the same: get `UNI-Design-AI-Guidelines/` in front of the tool, and make sure it reads [`START-HERE.md`](./UNI-Design-AI-Guidelines/START-HERE.md) **before** it generates anything.

Repo: `https://github.com/erikdandrade/uni-design-ai-guidelines`

If a value you need isn't documented, that's a gap — flag it rather than guessing, and open a PR against `main` if you can fill it in from the source Figma.

---

## Lovable

Lovable's GitHub integration is **export-only** — it creates and owns a repo mirroring the app it generates, and cannot import or read an external repo. So instead of connecting anything, you ask Lovable to **clone this repo into your project** as ordinary files.

### 1. Clone the guidelines into a subfolder

In your Lovable project chat:

> Clone `https://github.com/erikdandrade/uni-design-ai-guidelines` into a folder called `uni-design-ai-guidelines/` in this project. Don't touch anything at the project root. Then read `uni-design-ai-guidelines/UNI-Design-AI-Guidelines/START-HERE.md` and follow it for all UI work from now on.

**Use a subfolder, not the project root.** The repo has its own `README.md` and `.gitignore` at root; cloning to root will collide with your app's.

### 2. Add one line to Project Knowledge

Knowledge is the only channel that applies to *every* message, so it's what guarantees the guidelines aren't forgotten three prompts later:

> This project uses the UNI Design System. Before generating or modifying any UI, read `uni-design-ai-guidelines/UNI-Design-AI-Guidelines/START-HERE.md` and follow its mandatory question gate and rules. Never invent design values.

That's ~40 words instead of the whole prompt — the bulk specs now live in the project's files, so there's no need to spend the ~10,000-character Knowledge budget on them.

### 3. Verify the gate fires

Before trusting it, ask for a screen **without** mentioning the design system. Lovable should stop and ask you the build-context question rather than generating immediately. If it generates straight away, the guidelines aren't reaching it — re-check that step 2 saved.

### Updating

A clone is a **point-in-time snapshot**; nothing syncs. To refresh, ask Lovable to re-clone the repo over `uni-design-ai-guidelines/`.

---

## Claude Code

This repo ships as an installable **Claude Code plugin** (`.claude-plugin/marketplace.json` + the `uni-design` skill at `skills/uni-design/SKILL.md`), so no copy/paste is needed — the plugin's source is the repo itself, so the specs and `menu-icons/` travel with it and stay current with every commit.

One-time setup:

```
/plugin marketplace add erikdandrade/uni-design-ai-guidelines
/plugin install uni-design@uni-design-ai-guidelines
```

The skill triggers automatically on UI-generation requests ("build a screen", "implement this component", etc.) and tells Claude to read `UNI-Design-AI-Guidelines/START-HERE.md` before generating anything. Refresh with `/plugin marketplace update` to pick up the latest commit (no version is pinned in the manifest, so every commit to `main` counts as an update).

---

## Replit

Replit has no plugin/marketplace mechanism, and importing this repo directly would create a *separate* Repl rather than adding to an existing project. Instead, bring the guidelines in as a submodule of your own project:

```
git submodule add https://github.com/erikdandrade/uni-design-ai-guidelines.git uni-design-ai-guidelines
```

Then add a pointer to your own project's `replit.md` (the file Replit Agent reads each session for persistent context):

> For any UI/screen work, read `uni-design-ai-guidelines/UNI-Design-AI-Guidelines/START-HERE.md` before generating and follow its question gate and rules.

Update the submodule (`git submodule update --remote`) to pick up guideline changes.

---

## Claude.ai Projects

Claude.ai's GitHub connector syncs Project Knowledge **manually**, not live — there's no webhook/auto-sync yet.

1. In your Claude.ai Project, connect the GitHub integration and select this repo (or just the `UNI-Design-AI-Guidelines/` folder) as Knowledge.
2. Add this to the Project's custom instructions, so the rules apply to every conversation and not just ones where Claude happens to search Knowledge:
   > Before generating any UI, read `UNI-Design-AI-Guidelines/START-HERE.md` from Project Knowledge and follow its mandatory question gate and rules.
3. Click **Sync now** after any guideline update — it will not happen automatically.

**Unverified:** whether the GitHub connector indexes `.svg` file content as readable text or treats it as an opaque image attachment. Before relying on it to reproduce an icon's exact markup, test it once: sync, then ask Claude to quote back one icon's raw SVG source verbatim, and confirm it matches `menu-icons/`.

---

## Icon hosting & distribution

There is **no CDN or externally hosted copy** of `UNI-Design-AI-Guidelines/menu-icons/` — by design. The 20 section-logo SVGs are small XML text files, and every tool above gets them by bringing the actual repo content into its own context (clone, plugin bundle, submodule, or Knowledge upload), not by fetching a URL.

**Never reference an icon by external URL** — not even a `raw.githubusercontent.com` link. This matters most for Claude.ai Artifacts, which run under a strict Content Security Policy that blocks external image and network requests entirely; a linked-out icon simply won't render there.

Where the repo's files are physically present in the project (the Lovable and Replit paths above), importing the local `.svg` file is fine and usually cleaner. Where they aren't, or where the output must be self-contained, inline the raw `<svg>…</svg>` markup.
