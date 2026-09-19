# Toolbelt

Index cards for the external tools this harness leans on. Like `du-design`'s toolbelt: these are **pointers, not vendored copies** — what the tool is for, when to reach for it, how to install/use it. The tools install into the project or the agent environment, never into this repo.

| Card | Layer in the harness | Use it for |
|---|---|---|
| [`spec-kit.md`](spec-kit.md) | **Plan** | Turning a scoped spec into a plan + task list; structured spec-driven workflow |
| [`ponytail.md`](ponytail.md) | **Build / Review** | Stopping the agent from over-building; the "laziest senior dev" discipline |
| [`playwright.md`](playwright.md) | **Verify** | Proving a website/app actually works — driving a real browser, screenshots, E2E |
| [`hermes.md`](hermes.md) | **Runtime / Learn** | Running this harness inside Jerry's Hermes agent; its memory/skill loop |
| [`context7.md`](context7.md) | **Reference** | Feeding agents current, correct library docs so they stop guessing APIs |

## How they compose
spec-kit plans it → ponytail keeps the build minimal → Playwright proves it runs → Context7 keeps the APIs correct → Hermes (or Claude Code) is the runtime that remembers across sessions. Four layers, one system.

## Setup status
All installed 2026-09-19 (macOS, Claude Code):
- [x] **spec-kit** — `specify` CLI via `uv tool install` → `~/.local/bin/specify` (v1.0.9.dev). Run `specify init` in a project.
- [x] **ponytail** — Claude Code plugin `ponytail@ponytail` v4.10.0, user scope, enabled. Commands `/ponytail-review`, `/ponytail-audit`.
- [x] **Playwright** — global CLI v1.63 (`npm i -g playwright`) + Chromium browser installed. Add `@playwright/test` per project for E2E.
- [x] **Context7 MCP** — added at user scope: `context7 → https://mcp.context7.com/mcp` (in `~/.claude.json`).
- [x] **Hermes** reads `du-code/AGENTS.md` — loader skill in `orthogonal-work` profile (see [`hermes.md`](hermes.md)).
- [x] **Claude Code** auto-loads it — rule in `~/.claude/CLAUDE.md`.

Note: uv was installed to bootstrap spec-kit (`~/Library/Python/3.14/bin/uv`). ponytail/spec-kit are Claude-Code-first; in Hermes lean on the markdown principles + SOPs.
