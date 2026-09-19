# Web principles (websites & web apps)

For anything that runs in a browser. **The visual side lives in `../du-design` — read its `AGENTS.md` for look/feel/motion.** This file is the engineering side.

## Stack defaults
- Jerry's portfolio and similar sites use **Next.js** with **Resend** for email. Prefer the stack a project already uses; don't introduce a new framework without a reason.
- Static/simple sites don't need a framework — plain HTML/CSS/JS is fine and often better.

## Correctness & robustness
- Forms: validate input, show success and error states, never lose what the user typed on error.
- Every network call has a loading state and an error state. No infinite spinners.
- Handle the empty case (no data yet) and the failure case (fetch failed), not just the happy path.

## Don't ship broken basics
- Works on mobile widths, not just desktop.
- Links go somewhere; buttons do something; no dead placeholders in what you show Jerry.
- Fast: don't ship giant unoptimized images or block render on huge scripts.

## Security (see also [`security.md`](security.md))
- API keys and secrets stay server-side / in `.env`, never in client code (anything in the browser is public).
- Sanitize anything user-submitted before it's stored, displayed, or sent.
- Don't put personal data in URLs.

## Verify (see [`../sops/verification.md`](../sops/verification.md))
- Load it in a real browser, screenshot the key pages, click the main flow, screenshot the result.
- Check the live deployed URL, not just localhost, if it's deployed.
