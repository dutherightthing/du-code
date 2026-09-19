# Decisions log

Append-only history of what shipped and what was learned — the record behind [`your-eng-profile.md`](your-eng-profile.md). Per-project detail lives here so the profile stays lean. One short block per project at Capture time (Step 7); newest at top. Full artifacts (spec, code) stay in each project's own repo.

Format:
```
## YYYY-MM-DD — <project name> (<type>)
- Built: <one line>
- What changed from the first plan: <the useful signal>
- Verified by: <how we proved it worked>
- Preference learned (if any): <what got added to your-eng-profile.md>
```

---

## 2026-09-19 — du-code harness itself (meta)
- Built the engineering harness as a sibling to `du-design`: router (AGENTS.md), FDE scoping skill, principles (universal/web/workflows/security), SOPs (definition-of-done/verification/code-review/testing/git-hygiene), toolbelt cards (spec-kit/ponytail/playwright/hermes/context7).
- Decisions (all Jerry's picks): sibling folder (not a monorepo); tiered rigor (quick vs full); Claude-Code-first but portable markdown so Hermes can use it too; verification bar = "show me it running."
- Repo roles locked in: spec-kit = plan, ponytail = don't over-build, playwright = prove it runs, hermes = runtime + learning model. Context7 added as a suggested reference tool.
- Open: install the toolbelt tools; point Hermes at AGENTS.md; decide on a global "read du-code first" rule; push to GitHub.
