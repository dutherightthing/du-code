# Learning loop — how the harness gets smarter without bloating or drifting

The Capture step (Step 7) feeds two files. This SOP keeps them **lean** (no bloat), **stable** (one project can't hijack the defaults), and **fair** (a big, impactful project still carries weight). Read before writing anything back.

## The two files, two jobs
- **`decisions-log.md`** — append-only raw history. Grows forever, but stays cheap because every entry is tiny and links out.
- **`your-eng-profile.md`** — curated, lean, standing truth. Stays short because things only enter through the promotion gate below.

Never mix them: raw detail → log; durable standing rules → profile.

## Anti-bloat rules
1. **Log entries are ≤5 lines.** One block per project. Use the format in `decisions-log.md`.
2. **No artifacts in either file.** Specs, code, briefs, long output live in the *project repo*; the log links to them.
3. **Profile target: ~2 screens max.** If it's longer, run a consolidation pass.
4. **Consolidation pass** — every ~10 projects, or when the profile bloats: merge duplicates, delete stale/wrong facts, demote one-offs that never recurred, tighten wording. (The `consolidate-memory` skill is the same pattern if you want a hand.)

## Anti-drift: the promotion gate
Every learning is in one of three states. **Default is Logged.**

| State | Where it lives | Effect on future work |
|---|---|---|
| **Logged** | decisions-log only | A reference. Does *not* change how new projects are scoped. |
| **Candidate** | log, tagged `candidate` | Flagged as maybe-standing; watch for a repeat. |
| **Promoted** | profile | A standing default applied to future projects. |

**A learning is promoted to the profile ONLY when:**
- it **recurred across ≥2 projects**, OR
- Jerry **explicitly said "always do this / this is the standard now,"** OR
- it's a **hard fact** (stack, constraint, where secrets live) — facts are always promotable.

**One project alone never promotes a *preference*.** This is the anti-drift protection: a single loud project can't rewrite your defaults on its own.

**Contradictions:** log them plainly. They flip a promoted rule only if they *recur* or Jerry confirms the change. Don't silently overwrite a standing rule because one project went the other way.

## Weighting impactful projects (fairly)
Tag each log entry `impact: high | med | low`. **Impact changes how much we *learn* from a project — not its power to override defaults.**
- **High-impact** → write a richer reference note (in `references/past-projects/`), and **cite it first** during future scoping ("last big project like this, we did X; here's what broke").
- It still needs recurrence or Jerry's confirmation to become a global default.
- **Exception:** a hard fact/standard set by a big project ("we've standardized on X now") promotes immediately — because that's Jerry confirming a new standard, not project size deciding for him.
- **Facts vs. tastes:** facts promote freely; tastes/preferences need recurrence. This stops a big, emotionally-charged project from overwriting defaults with a one-off mood.

## The Capture procedure (Step 7, concretely)
1. Ask Jerry 1–2 questions: *what changed from the first plan? anything you'll always want this way?*
2. Write the ≤5-line block to `decisions-log.md` (newest on top), with `impact:` tag and `verified by:`.
3. Decide state: does it meet the promotion gate? If yes → add/update one lean line in `your-eng-profile.md`. If no → leave it Logged (or mark `candidate`).
4. If high-impact → add a `references/past-projects/<name>.md` note.
5. Only write **confirmed** learnings. When unsure, log as candidate, don't promote.

## Runtime note (Hermes / Claude Code)
Both runtimes read *and write back to these same files* — this repo is the shared brain. Whichever agent captures a learning writes it here so the other sees it. They do not sync automatically; the shared folder is the integration point. See [`../toolbelt/hermes.md`](../toolbelt/hermes.md).
