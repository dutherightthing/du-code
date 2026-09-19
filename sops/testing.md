# Testing — what to test and when

Don't test everything; test what would hurt if it broke. For Jerry, a test's real value is that **he can re-run it later to prove things still work** without reading code.

## The tiered rule
- **Quick tasks:** no formal tests. Just run it on real input and show the output (see [`verification.md`](verification.md)).
- **Full / real projects:** at least a **smoke test** of the core path — the one thing that must work. Add more only where breakage would be costly or silent.

## What deserves a test
- The core happy path (the reason the thing exists).
- Anything involving money, data changes, dates, or math — bugs here are silent and expensive.
- A bug you just fixed — add a test so it can't come back.
- The failure path for external calls (API down, empty input).

## What to skip
- UI pixel details (that's `du-design`'s taste pass + a Playwright screenshot).
- Trivial glue code, throwaway scripts, one-off explorations.
- Testing the framework itself.

## How, by type
- **Websites/apps:** Playwright for the key user flow (load → interact → see result). Doubles as verification.
- **Scripts/logic:** a tiny unit test on the core function with a couple of real cases + one edge case. Use whatever the project's language ships with (pytest, node's built-in test runner, etc.).
- **Agents:** one scripted end-to-end run with a known input and an expected-shape output.

## Always
- Tests must actually run and pass before "done" — paste the passing output.
- Leave the run command in the README as a ```bash block so Jerry can re-run it.
- If a test is flaky or you skipped coverage somewhere, say so.
