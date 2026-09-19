# Context7 — current library docs for agents

**Repo:** https://github.com/upstash/context7 · An MCP server that feeds coding agents **up-to-date, version-correct documentation** for libraries and frameworks, pulled on demand.

## Why it's here
Jerry's #1 stated concern is that agents can "pull context, pull references, and copy correctly." A big failure mode is agents inventing APIs that don't exist or are out of date (especially on fast-moving stacks like Next.js). Context7 fixes exactly that: the agent fetches the real current docs for the exact library version instead of guessing from training data.

## Use it for
- Any project on a library the agent might be hazy or outdated on (Next.js, Resend, a new SDK, an API client).
- When an agent starts hallucinating method names or config that doesn't work — pull the real docs.

## How to use (Claude Code)
- Add it as an MCP server (per the repo README). Once connected, ask the agent to "use context7" or reference a library and it pulls current docs into context.
- Complements, doesn't replace, the official docs — good for correctness on the fly during a build.

## Fits the harness at
**Build** and **Reference.** Pair with ponytail: Context7 tells you the *right* API to use, ponytail keeps you from writing more than you need of it.

## Note
This is a suggested addition (not one of the four repos Jerry named) — install only if the "wrong/outdated API" problem shows up. Low cost, high leverage for a non-coder who can't spot a hallucinated method.
