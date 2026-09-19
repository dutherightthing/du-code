# du-code

An engineering harness for Jerry's code projects — websites, agents, workflows, scripts, anything with code. Sibling to [`du-design`](../du-design) (which handles anything visual).

## Why this exists

Jerry is not a coder. He describes what he wants in outcomes, can't read the code to check it, and won't always know what to ask for. Default agents make this worse: they take a vague request and produce one unverified guess.

`du-code` turns any agent into a **Forward-Deployed Engineer** who covers those gaps: scopes the problem, grills Jerry on the specs he didn't think to give, builds the smallest thing that works, and **proves it runs** before saying done — the same way, every time, no matter which agent or project.

## How an agent uses it

**Start every code project by reading [`AGENTS.md`](AGENTS.md).** It's the router. The loop it enforces:

**Scope → Plan → Build → Verify → Review → Capture**

1. **Scope** — [`00-scoping/SKILL.md`](00-scoping/SKILL.md) interviews Jerry and writes a `spec.md`.
2. **Plan** — spec → task list (spec-kit style).
3. **Build** — smallest thing that works (ponytail discipline) + [`principles/`](principles/).
4. **Verify** — prove it runs; show Jerry evidence ([`sops/verification.md`](sops/verification.md)).
5. **Review** — self-review + [`sops/definition-of-done.md`](sops/definition-of-done.md).
6. **Capture** — write learnings back to profile + decisions-log.

## Two layers (don't mix them)

- **The Engineering Brain** — portable markdown, works in any agent: [`AGENTS.md`](AGENTS.md), [`00-scoping/`](00-scoping/), [`principles/`](principles/), [`sops/`](sops/).
- **The Toolbelt** — pointers to external tools we install into projects/agents, not vendored: [`toolbelt/`](toolbelt/).

## Layout

```
du-code/
├── AGENTS.md            ← the router (agents read this first)
├── CLAUDE.md            ← points Claude at AGENTS.md
├── your-eng-profile.md  ← Jerry's standing engineering constraints (grows over time)
├── decisions-log.md     ← dated record of what shipped + what was learned
├── 00-scoping/          ← ⭐ the FDE intake skill — run first
├── principles/          ← rules by project type (universal, web, workflows, security)
├── sops/                ← the harnesses (definition-of-done, verification, code-review, testing, git)
├── toolbelt/            ← index cards: spec-kit, ponytail, playwright, hermes, context7
└── references/          ← annotated past projects (learning inputs)
```

## The tools it composes
[spec-kit](https://github.com/github/spec-kit) plans it · [ponytail](https://github.com/DietrichGebert/ponytail) keeps it minimal · [Playwright](https://github.com/microsoft/playwright) proves it runs · [Context7](https://github.com/upstash/context7) keeps APIs correct · [Hermes](https://github.com/NousResearch/hermes-agent) / Claude Code run it and remember across sessions. See [`toolbelt/`](toolbelt/).

## Using it — just point an agent here

No installs required for the core (it's all markdown). Say:

> "Read `du-code/AGENTS.md` before starting, and follow it."

The toolbelt tools (spec-kit, ponytail, Playwright, Context7) are optional power-ups — install them per their cards when a project benefits.

## Setup checklist
- [ ] Install spec-kit, ponytail, Playwright, Context7 (see [`toolbelt/README.md`](toolbelt/README.md)) — optional but recommended.
- [ ] Point Hermes at `du-code/AGENTS.md` so both runtimes share the harness.
- [ ] Consider a global rule: "For code projects, read du-code/AGENTS.md first" (like the du-design one).
- [ ] Push to GitHub when ready (Jerry's call — publishing is his to trigger).
