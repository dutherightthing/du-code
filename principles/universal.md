# Universal engineering principles

Apply to every code project regardless of type. Read this first, then the medium-specific file.

## For a non-coder client
- **Translate, don't jargon.** Every explanation lands in plain English. "I added a form that emails you when someone submits it" — not "wired up a POST handler."
- **Surface decisions, don't bury them.** When there's a real choice (a cost, a tradeoff, a risk), name it and recommend one — don't decide silently and don't dump it on him undecided.
- **You are the safety net.** Jerry can't see bugs, cost, or risk. If you don't catch it, no one does.

## Building
- **Smallest thing that works first.** Ship the core, then add. Don't build for imagined future needs (ponytail discipline).
- **Reuse before you write.** Check: does the language, an installed dependency, or existing project code already do this?
- **Make it obvious, not clever.** Readable beats impressive. Comment the *why*, not the *what*.
- **Handle failure.** Anything external (network, API, file, user input) will fail eventually — fail clearly and safely.
- **Consistency with the existing project** beats your personal preference. Match the surrounding style.

## Never
- Hardcode secrets. Ever. `.env` + `.gitignore`, and Jerry enters his own.
- Automate spending money, sending messages, posting publicly, or deleting data without explicit per-action confirmation.
- Say "done" without showing it running.
- Leave a silent gap — unfinished or unsure things get said out loud.

## Cost & dependencies
- Flag anything that costs money (paid APIs, hosting, per-call fees) before building on it.
- Prefer few, well-known dependencies over many obscure ones. Each dependency is a thing that can break.
