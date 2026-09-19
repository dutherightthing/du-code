# Definition of Done

Nothing is "done" until every box that applies is checked. This is the gate. Because Jerry can't read the code, "done" means **demonstrated**, not claimed.

## Every task
- [ ] It does what `spec.md` said (re-read the "Done looks like" line).
- [ ] **It runs.** You executed it / loaded the page / ran the test — and you're showing Jerry proof (see [`verification.md`](verification.md)).
- [ ] No obvious way for it to lose data, spend money, or expose private info without Jerry knowing.
- [ ] Errors are handled — it fails with a clear message, not a silent crash or a wrong answer.
- [ ] You explained, in plain English, what it does and how Jerry uses it.
- [ ] Secrets (keys, passwords) are in `.env` / entered by Jerry — never hardcoded, never committed.
- [ ] Work is committed to git so it can't be lost (see [`git-hygiene.md`](git-hygiene.md)).

## Full-tier / real projects, additionally
- [ ] Self-review pass done ([`code-review.md`](code-review.md)).
- [ ] The core path has at least a smoke test or a Playwright check that Jerry can re-run.
- [ ] A short `README` says what it is, how to run it, and what could break.
- [ ] Anything left unfinished is written down (in the README or spec's "Open questions"), not left silent.
- [ ] Learnings captured back to `du-code` (decisions-log, and profile if it's a standing preference).

## The honesty rule
If something doesn't work, isn't finished, or you skipped a step — **say so plainly**, with the evidence. "Tests fail here's the output" / "I couldn't verify X" is always better than a confident "done." Jerry is trusting you precisely because he can't check.
