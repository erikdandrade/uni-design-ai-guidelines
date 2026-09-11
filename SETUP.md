# Setup — connecting your AI tool to the UNI Design AI Guidelines

This repo is **read-only for consumers**, hosted on a personal GitHub account (`erikdandrade`) and maintained by its owner. Pick your tool below. In every case, the goal is the same: point the tool at `UNI-Design-AI-Guidelines/` (and `AI-builder-prompt.md` specifically) so it builds from documented values instead of inventing its own.

If a value you need isn't documented, that's a gap — flag it rather than guessing, and open a PR against `main` if you can fill it in from the source Figma.

---

## Lovable

Lovable's Knowledge panel caps at ~10,000 characters, so the full specs can't be pasted in directly.

1. **GitHub two-way sync**: connect this repo (`erikdandrade/uni-design-ai-guidelines`) to your Lovable project via its GitHub integration. This gives Lovable the full `UNI-Design-AI-Guidelines/` folder, including `menu-icons/`.
2. **Knowledge base entry**: paste the contents of [`UNI-Design-AI-Guidelines/AI-builder-prompt.md`](./UNI-Design-AI-Guidelines/AI-builder-prompt.md) verbatim (it fits under the char cap) plus a line telling Lovable to read the relevant `/UNI-Design-AI-Guidelines/*.md` file for the area it's building.

Updates to the repo arrive automatically through the sync; the Knowledge base entry only needs updating if `AI-builder-prompt.md`'s rules themselves change.

---

## Claude Code

This repo ships as an installable **Claude Code plugin** (`.claude-plugin/marketplace.json` + the `uni-design` skill at `skills/uni-design/SKILL.md`), so no copy/paste is needed — the plugin's source is the repo itself, so the specs and `menu-icons/` travel with it and stay current with every commit.

One-time setup:

```
/plugin marketplace add erikdandrade/uni-design-ai-guidelines
/plugin install uni-design@uni-design-ai-guidelines
```

The skill triggers automatically on UI-generation requests ("build a screen", "implement this component", etc.) and tells Claude to read `UNI-Design-AI-Guidelines/AI-builder-prompt.md` and its companion files before generating anything. Refresh with `/plugin marketplace update` to pick up the latest commit (no version is pinned in the manifest, so every commit to `main` counts as an update).

---

## Replit

Replit has no plugin/marketplace mechanism and importing this repo directly would create a *separate* Repl rather than adding to an existing project. Instead, bring the guidelines in as a submodule of your own project:

```
git submodule add https://github.com/erikdandrade/uni-design-ai-guidelines.git uni-design-ai-guidelines
```

Then add a pointer to your own project's `replit.md` (the file Replit Agent reads each session for persistent context), mirroring the pattern this repo uses in its own `CLAUDE.md`:

> For any UI/screen work, read `uni-design-ai-guidelines/UNI-Design-AI-Guidelines/*.md` before generating — start with `AI-builder-prompt.md` and follow its rules.

Update the submodule (`git submodule update --remote`) to pick up guideline changes.

---

## Claude.ai Projects

Claude.ai's GitHub connector syncs Project Knowledge **manually**, not live — there's no webhook/auto-sync yet.

1. In your Claude.ai Project, connect the GitHub integration and select this repo (or just the `UNI-Design-AI-Guidelines/` folder) as Knowledge.
2. Add the contents of `AI-builder-prompt.md` to the Project's custom instructions so the anti-invention rules apply to every conversation in the Project, not just ones where Claude happens to search Knowledge.
3. Click **Sync now** after any guideline update — it will not happen automatically.

**Unverified:** whether the GitHub connector indexes `.svg` file content as readable text or treats it as an opaque image attachment. Before relying on it to reproduce an icon's exact markup, test it once: sync, then ask Claude to quote back one icon's raw SVG source verbatim, and confirm it matches `menu-icons/`.

---

## Icon hosting & distribution

There is **no CDN or externally hosted copy** of `UNI-Design-AI-Guidelines/menu-icons/` — by design. The 20 section-logo SVGs are small XML text files (under 7KB each), and every tool above gets them by bringing the actual repo content into its own context (full sync, plugin bundle, submodule, or Knowledge upload), not by fetching a URL.

**Always inline the raw `<svg>…</svg>` markup directly into generated code. Never reference an icon by external URL** — even a `raw.githubusercontent.com` link. This matters most for Claude.ai Artifacts, which run under a strict Content Security Policy that blocks external image and network requests entirely; a linked-out icon simply won't render there. Inlining works uniformly across all four tools, so it's the only pattern worth using anywhere.
