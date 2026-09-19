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
Fill in as you install each (leave a dated line):
- [ ] spec-kit —
- [ ] ponytail —
- [ ] Playwright —
- [ ] Context7 MCP —
- [x] Hermes reads `du-code/AGENTS.md` — 2026-09-19, loader skill in `orthogonal-work` profile (see [`hermes.md`](hermes.md))
- [x] Claude Code auto-loads it — 2026-09-19, rule added to `~/.claude/CLAUDE.md`
