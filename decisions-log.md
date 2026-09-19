# Decisions log

Append-only history of what shipped and what was learned — the record behind [`your-eng-profile.md`](your-eng-profile.md). Per-project detail lives here so the profile stays lean. One short block per project at Capture time (Step 7); newest at top. **Keep each block ≤5 lines. No artifacts here** — specs/code/briefs stay in the project repo, link to them.

Bloat, drift, and weighting are governed by [`sops/learning-loop.md`](sops/learning-loop.md). Key rule: a preference only moves into the profile if it recurs (≥2 projects) or Jerry says "always" — one project never rewrites the defaults. `impact:` controls how much we *learn* from a project (richer reference note, cited first), not its power to override.

Format:
```
## YYYY-MM-DD — <project name> (<type>) · impact: high|med|low
- Built: <one line>
- Changed from first plan: <the useful signal>
- Verified by: <how we proved it ran>
- Learned: <preference/fact> → [logged | candidate | promoted to profile]
```

---

## 2026-09-19 — du-code harness itself (meta) · impact: high
- Built the engineering harness as a sibling to `du-design`: router (AGENTS.md), FDE scoping skill, principles (universal/web/workflows/security), SOPs (definition-of-done/verification/code-review/testing/git-hygiene), toolbelt cards (spec-kit/ponytail/playwright/hermes/context7).
- Decisions (all Jerry's picks): sibling folder (not a monorepo); tiered rigor (quick vs full); Claude-Code-first but portable markdown so Hermes can use it too; verification bar = "show me it running."
- Repo roles locked in: spec-kit = plan, ponytail = don't over-build, playwright = prove it runs, hermes = runtime + learning model. Context7 added as a suggested reference tool.
- Open: install the toolbelt tools; point Hermes at AGENTS.md; decide on a global "read du-code first" rule; push to GitHub.
