# Hermes Agent — a runtime + the learning model

**Repo:** https://github.com/NousResearch/hermes-agent · "The agent that grows with you." Self-improving autonomous agent: creates skills from experience, improves them in use, searches its own past conversations, builds a model of you across sessions. Jerry has it installed (CLI + app).

## Two roles in `du-code`

### 1. A runtime that should obey this harness
`du-code` is plain markdown on purpose so Hermes can follow it too, not just Claude Code. To use it in Hermes: point Hermes at `du-code/AGENTS.md` as a standing instruction / skill so it runs the same Scope → Plan → Build → Verify → Review → Capture loop. The plugin-based tools (spec-kit, ponytail) are Claude-Code-first; in Hermes, lean on the markdown principles + SOPs, which are engine-agnostic.

### 2. The model for our own learning loop
Hermes's self-evolution (skills from experience, memory across sessions) is exactly the "iterative learning" Jerry wants. Our version of it is lower-tech but the same idea:
- `decisions-log.md` = the record of what shipped and what worked (like Hermes remembering past conversations).
- `your-eng-profile.md` = the deepening model of Jerry (like Hermes's model of the user).
- The **Capture** step in every project = the improvement loop.

See also **hermes-agent-self-evolution** (DSPy + GEPA optimization of skills/prompts) and the community **awesome-hermes-agent** directory — useful if we later want Hermes to auto-improve `du-code` skills.

## When to reach for Hermes
- Long-running / autonomous jobs that should live on a cheap always-on VM, not Jerry's laptop (Hermes runs on a $5 VPS, reachable from Telegram).
- Work where cross-session memory of Jerry's preferences matters most.

## Installed as a Hermes skill (done 2026-09-19)
- A thin **loader skill** lives at `~/.hermes/profiles/<profile>/skills/software-development/du-code/SKILL.md`. It doesn't copy the harness — it tells Hermes to read `/Users/jerrydu/du-code/AGENTS.md` and follow it, keeping this repo the single source of truth.
- Installed in the **`orthogonal-work`** profile (the active one). **Hermes skills are per-profile** — if Jerry switches profiles (`hermes` profile commands), re-add the loader to that profile's `skills/software-development/du-code/`.
- Verify with `hermes skills list | grep du-code` (should show `local · enabled`). `hermes skills inspect` only checks remote registries, so it won't find a local skill — that's expected.
- On a Docker/cloud Hermes where `/Users/jerrydu/du-code` doesn't exist, push du-code to GitHub and have the loader clone it first (the SKILL.md already says to).

## Keep in mind
- Whichever runtime is used, the harness is the same. Don't let Hermes and Claude Code learn different things — the profile + decisions-log in *this repo* are the shared brain both write back to.
- Claude Code auto-loads du-code via a rule in `~/.claude/CLAUDE.md` ("read du-code/AGENTS.md first on code projects"). Hermes auto-loads it via the skill above.
