# UNI Design AI Guidelines

This repo is the **UNI Design System reference set** — a curated set of Markdown specs and icon assets used to steer **AI-assisted UI generation** for Unifonic products across whichever tool a team happens to build with.

It is not an application. There is nothing to run or deploy here.

- **What's inside, and how it's organized:** see [`CLAUDE.md`](./CLAUDE.md) — the map of this repo and its running session log.
- **If you are an AI builder:** start at [`UNI-Design-AI-Guidelines/START-HERE.md`](./UNI-Design-AI-Guidelines/START-HERE.md). Read it before generating any UI.
- **How to connect your AI tool to these guidelines:** see [`SETUP.md`](./SETUP.md) for per-tool instructions (Lovable, Claude Code, Replit, Claude.ai Projects).
- **The specs themselves:** [`UNI-Design-AI-Guidelines/`](./UNI-Design-AI-Guidelines/).

## Quick start with Lovable

Paste this into your Lovable project chat:

> Clone `https://github.com/erikdandrade/uni-design-ai-guidelines` into a folder called `uni-design-ai-guidelines/` in this project. Don't touch anything at the project root. Then read `uni-design-ai-guidelines/UNI-Design-AI-Guidelines/START-HERE.md` and follow it for all UI work from now on.

Then add the one-line Knowledge entry from [`SETUP.md`](./SETUP.md#lovable) so the rules apply to every message, not just the first — and verify the gate actually fires before you trust it. Full instructions and the verification step are in `SETUP.md`.

## What's inside

| | |
|---|---|
| `START-HERE.md` | Builder entry point — the mandatory question gate, core rules, file index |
| `AI-builder-prompt.md` | The complete rule set and gate, in full |
| `Design-guidelines.md` | Typography, spacing, radius, elevation, color usage, component specs |
| `Color-tokens.md` | Every color token → hex, by role |
| `Layout.md` | Shell structure and layout modes |
| `Side-navigation.md` | The `uni-menu` side nav, including the Console → Section content map |
| `Top-bar.md` | The Main Top Bar / Local Header pair and its visibility matrix |
| `menu-icons/` | 20 section-logo SVGs + a Section → filename manifest |

The specs are authored from the source Figma file and written for LLM consumption: raw hex build values, with token names given to identify intent rather than to be implemented as infrastructure. The goal is visual fidelity in lightweight prototypes.

## Access model

This repo is public and read-only for consumers. If you find a gap or an error in a spec, open a PR against `main` rather than forking a divergent copy — every tool listed in `SETUP.md` reads directly from this repo, so a fix here reaches everyone.
