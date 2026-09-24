# Jev selector — reviewed search-only API discovery demo

- Project: `/Users/jerrydu/Github Repositories/jev-selector`
- Live site: https://findtherightapi.vercel.app
- Repository: https://github.com/Angularious/jev-selector
- Review date: 2026-09-22
- Status: reviewed working state committed and pushed as `e1479cf` on `Angularious/jev-selector` `main`

## Product boundary

Visitors describe a goal, local retrieval finds plausible public Orthogonal endpoints, and Jev judges the supplied candidates. The site displays provider and endpoint information, pricing, deterministic code snippets, and an Orthogonal CTA. It never runs provider endpoints, requests visitor keys, handles payments, or spends Orthogonal credits.

## Architecture worth reusing

1. Keep an explicit public catalog snapshot rather than asking the model to invent or retrieve providers.
2. Use deterministic broad retrieval before Jev. Jev judges supplied candidates and returns typed IDs; it is not the catalog, execution layer, or code generator.
3. Validate every returned probability and endpoint ID against the exact candidate set. Invalid IDs are rejected rather than silently mapped to a fallback endpoint.
4. Generate REST snippets deterministically from catalog schemas and required parameters.
5. Separate the animated physics presentation from the stable results carousel and detail state.
6. Keep vendor credentials server-side and place cost controls before the paid model request.

## Review findings and fixes

- The launch plan specified abuse and budget controls, but the route had none. Added 30 requests/browser/hour, 1,000 requests/IP/hour, a 10,000 paid-selection/day circuit breaker, a conservative 16 KB serialized Jev-input ceiling, hashed short-lived rate keys, and a two-hour HttpOnly abuse-control cookie.
- No-match queries originally bypassed throttling while still writing query-log rows. Split hourly request throttling from the paid daily allowance so every valid query is throttled and only imminent Jev calls consume the daily cap.
- Network timeouts escaped as generic 500s. TypeSafe failures now return controlled 502 responses without automatic retry.
- Invalid Jev endpoint choices silently fell back to endpoint zero. They are now rejected.
- `.env.example` was accidentally re-ignored by a later `.env*` rule. The ignore order was corrected and the rate-limit secret documented.
- Physics body dimensions stayed desktop-sized after crossing the mobile breakpoint. Resize handling now scales collision bodies with the DOM tiles.
- Password changes did not invalidate older dashboard sessions. Sessions now carry an issue time checked against the stored password-update time.
- Catalog logo tests checked path strings but not actual files. Tests now verify every referenced local asset exists.

## Privacy and security pattern

- Raw searches are retained for 30 days only because the owner explicitly chose a private query dashboard and the public privacy page discloses it.
- Query logs contain no IP or visitor ID. Abuse controls store keyed hashes separately and delete expired rows.
- Admin pages use signed HttpOnly cookies, no-store/noindex headers, password hashing, login throttling, and password-change session revocation.
- Do not reuse the prompt-retention decision silently; make it an explicit product/privacy choice for each project.

## Gap closure

- Added five recorded-response route tests covering success, out-of-set endpoint rejection, TypeSafe timeout, rate-limit rejection, and local no-match behavior without a paid call.
- Added a daily GitHub Actions catalog refresh with manual dispatch, catalog/test/build validation, stable curated-logo reuse, and commit-on-change behavior.
- Added an expandable, scrollable list of every public endpoint on each result card. Selecting an endpoint updates its price and cURL locally.
- Added deterministic Playwright coverage and screenshot baselines for initial and expanded result states at 1440×960 and 390×844. Tests also check page overflow, endpoint count, snippet updates, and complete result clearing.

## Verification evidence

- `npm test`: 16 tests passed, including retrieval regressions, route contracts, budget behavior, provider coverage, and local logo existence.
- `npm run test:e2e`: four Chrome tests passed against desktop/mobile screenshot baselines.
- `npm run lint`: passed.
- `npm run build`: passed on Next.js 16.
- `npm audit`: zero production or development vulnerabilities after upgrading Vitest.
- Gitleaks: nine commits and the working tree scanned with no leaks.
- Direct Neon limiter exercise: requests 1–30 allowed and request 31 blocked for the same browser token; no Jev/provider call was made for this test.

## Automation activation

- The daily catalog workflow is active on `main`. A manual dispatch completed checkout, install, live catalog sync, 16 tests, production build, and the no-change commit path successfully on 2026-09-23.

Do not copy the current component wholesale. Reuse these boundaries, tests, and failure lessons; re-review any extracted implementation in its new context.
