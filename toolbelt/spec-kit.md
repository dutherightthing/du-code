# spec-kit — the planning backbone

**Repo:** https://github.com/github/spec-kit · GitHub's open-source toolkit for Spec-Driven Development. Works with Claude Code, Copilot, Gemini CLI, Cursor.

## What it is
A structured workflow that turns an idea into a spec, a plan, and a task list *before* code — the exact FDE discipline this harness is built around. Its stages:
- **constitution** — the project's standing rules (maps to `du-code` principles + `your-eng-profile.md`).
- **/specify** — the *what* and *why*, no implementation detail (maps to our `00-scoping` → `spec.md`).
- **/plan** — the technical approach.
- **/tasks** — an ordered, checkable task list.
- **implement** — build against the tasks.

## When to use it
- **Full-tier projects.** For quick tasks, our lightweight scoping is enough — don't over-ceremony a one-file script.
- When a project will be built on over time and you want the spec/plan to persist and stay in sync.

## How it fits `du-code`
Our `00-scoping/SKILL.md` already does the interview and produces `spec.md`. spec-kit gives that spec a formal downstream (`/plan`, `/tasks`) and keeps everything referencing one source of truth. Use our scoping to *get the spec right with Jerry*, then spec-kit's structure to drive planning and execution.

## Install / use (Claude Code)
It ships as slash commands via its CLI (`specify`). Install per the repo README, then the `/specify`, `/plan`, `/tasks` commands appear in Claude Code. Point its "constitution" at `du-code` principles so the two don't contradict.

## Don't
- Don't run the full ceremony on throwaway work.
- Don't let spec-kit's spec and our `spec.md` drift — pick one as canonical per project (recommend spec-kit's once installed, with our scoping feeding it).
