# FindTheRightAPI / Jev selector

Last reviewed: 2026-09-24  
Live site: https://www.findtherightapi.com  
Source: https://github.com/Angularious/jev-selector  
Project handoff: `PROJECT_LEARNINGS.md` in the source repository

## What it is

A public, search-only discovery surface for Orthogonal. A visitor describes a job, sees matching APIs and endpoints, compares prices and code snippets, and continues to Orthogonal. The site does not call provider endpoints, take payments, or accept visitor API keys.

Jev is the judgment layer, not the retrieval engine. Deterministic code retrieves a bounded candidate set from the checked-in Orthogonal catalog, then Jev judges provider relevance and endpoint fit.

## Shipped stack

- Next.js 16, React 19, TypeScript, Zod
- Matter.js for the interactive API-logo pile
- GSAP for pile-to-carousel and detail transitions
- Neon for anonymous query logs and rate-limit state
- Vercel for previews, production, and aggregate Web Analytics
- Vitest for retrieval and route tests; Playwright exists for browser coverage

As of this review, production searches 69 APIs and 1,031 endpoints after excluding retired Brand.dev. The raw snapshot contains 70 APIs and 1,044 endpoints. These figures will drift with catalog refreshes and must be recalculated rather than repeated as permanent facts.

## Retrieval pattern worth reusing

The useful architecture is a guarded funnel:

1. Keep a validated, checked-in catalog snapshot so user searches do not depend on a live catalog request.
2. Retrieve deterministically from names, descriptions, endpoint schemas, parameters, capability hints, typo aliases, intent aliases, and budget signals.
3. Limit provider dominance before the model stage. This app uses 80 endpoints, five endpoints per provider, and the first 18 unique providers.
4. Use BM25 only as a zero-result fallback. A global lexical blend looked more sophisticated but displaced strong weather, virtual-machine, and speech matches.
5. Have Jev judge the providers it actually receives, then select endpoints, with a relevance floor and request-size batching.
6. If retrieval is empty, let Jev classify the capability and retry retrieval once. Do not let a model fabricate providers outside the catalog.

The evaluation must inspect the exact 18-provider production window. Earlier tests passed because a desired provider was somewhere in the 80-endpoint shortlist even though Jev never received it.

## Search lessons

- Separate retrieval recall from model judgment. The model cannot select a provider that deterministic retrieval omitted.
- Expand reusable capability vocabulary instead of hardcoding complete prompts. People express the same intent as fragments, typos, questions, and broad requests.
- Preserve several endpoint types per provider. Speech-to-text and text-to-speech, or search and fetch, are materially different even under one brand.
- Allow a precise query to return only one or two strong matches. Filling a result quota degrades trust.
- Give genuinely broad prompts a deliberately diverse general-purpose set rather than pretending the query was specific.
- Turn anonymous no-result queries into a regression corpus. Cluster repeated intents, add representative tests, and fix clusters rather than isolated strings.
- Add negative checks with every recall improvement. Email or scraping vocabulary should not cause unrelated voice APIs to appear.

## Catalog lessons

- Count the searchable, post-filter catalog rather than the raw JSON. Retired Brand.dev explained a prior count mismatch.
- Compare the current checkout, `origin/main`, and the upstream catalog before concluding data is missing. A stale checkout produced an obsolete count.
- Represent one provider with several base URLs as one upstream API group. TinyFish was initially exposed as implementation-specific children; the canonical group now presents one provider with eight Agent, Research, Search, and Fetch endpoints.
- Fail synchronization on duplicate slugs and on falling below a public numeric claim. The sync currently enforces the 1,000+ searchable-endpoint floor.
- Review generated metadata and asset diffs because a catalog refresh can bring unrelated upstream churn.

## UI and motion lessons

- A convincing result transition requires stable logo identity. Keep the Matter.js world mounted and move the matching DOM elements from their pile coordinates into carousel targets.
- Do not replay the pile entrance on submit or resize. ResizeObserver should update bounds and targets in place.
- Physics, stacking, opacity, pointer targets, and card geometry all need dedicated mobile testing. Desktop fixes did not automatically prevent background logos from leaking through mobile result cards.
- Use authentic square provider icons. Generic symbols, blank tiles, low-resolution favicons, and duplicate pile entries were visible quality failures.
- Closing a detail card must also clear the selected and dimmed state.
- Browser recordings are poor substitutes for interaction review when capture timing itself changes the feel. Test motion live at desktop, mobile, and during resizing.

## Privacy and operational lessons

- The exact anonymous prompt, results, diagnostics, safety classification, and optional no-result message are stored when Neon is configured and retained until manually deleted.
- Query logs contain no IP address, account ID, or visitor ID. Short-lived abuse hashes and a temporary cookie are separate from the research log.
- Raw prompts are not sent to Vercel Analytics. Aggregate visitor analytics can still appear beside query summaries in the admin dashboard.
- Keep TypeSafe, database, and admin credentials server-side and separate across preview and production.
- Production verification stays search-only. Never execute a provider endpoint or spend Orthogonal credits while testing this application.

## What changed from the first plan

- The original plan assumed a much smaller static catalog contract; authenticated catalog sync and a 1,000+ endpoint guard replaced it.
- The original privacy plan said raw prompts would not persist. The shipped product explicitly retains anonymous prompts until deletion and documents that at `/privacy`.
- A simple keyword layer grew into deterministic semantic rules plus a narrow lexical fallback because real user prompts exposed large vocabulary gaps.
- The first motion implementations remounted or reset the pile and produced janky transitions. Stable scene ownership and element identity were the important fix.
- TinyFish was initially represented as several records because of different base URLs. The durable fix was an upstream group, not frontend deduplication alone.

## Verification evidence at closeout

- 105 tests passed and one was skipped.
- ESLint and production build passed.
- The opt-in live Jev suite passed.
- Catalog regression fixtures covered 28 representative queries against the actual 18-provider Jev window.
- Production displayed the 1,000+ copy, contained only canonical `tinyfish`, and returned TinyFish for a live search without executing a provider endpoint.

## Remaining external work

- The canonical TinyFish group remains marked unverified upstream.
- Orthogonal's TinyFish article mentions a Browser surface that is absent from the current group. Add the endpoint if it exists or correct the article.
- Treat this as a pattern reference, not a code template. The selector accumulated many rounds of product-specific vocabulary and motion tuning; reuse the architecture and tests selectively.
