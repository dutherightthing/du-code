# ponytail — the anti-over-building discipline

**Repo:** https://github.com/DietrichGebert/ponytail · "Makes your AI agent think like the laziest senior dev in the room. The best code is the code you never wrote."

## What it is
A plugin + ruleset for AI coding agents that inserts a **decision ladder before code generation**. Reported to cut generated code ~54% (up to 94%) while keeping all safety/validation/accessibility guards. Works across 20+ agents (Claude Code, Cursor, Copilot, Cline, Aider, Zed, etc.).

## The ladder (runs before writing anything)
1. Is this even needed?
2. Does it already exist in this project?
3. Is it in the standard library?
4. Is it a native language/framework feature?
5. Is it in a dependency already installed?
6. Is it a one-liner?
7. *Only then* write minimal code.

This is baked into our `principles/universal.md` and `sops/code-review.md` (the "simplicity" axis). ponytail enforces it automatically.

## When to use it
- **Always on for building.** Over-building is the default agent failure mode; this counteracts it. Especially valuable when Jerry can't tell bloated code from lean code.

## Commands
- `/ponytail-review` — audit a diff for unnecessary code.
- `/ponytail-audit` — scan the whole repo.
- `/ponytail-debt` — defer refactors into a ledger instead of doing them now.
- `/ponytail ultra` — max intensity.

## Install (Claude Code)
Plugin marketplace: `/plugin install ponytail` (per the repo README). Instruction-only fallback exists for other agents (copy its rule files into the project). Since we're Claude-Code-first, use the plugin.

## Caution
- Laziness is about *quantity*, not corner-cutting. It preserves guards — but still run our `verification.md`; less code you didn't verify is still unverified code.
