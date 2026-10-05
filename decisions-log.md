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

## 2026-10-05 — jev-selector nightly catalog refresh (CI fix) · impact: med
- Built: PR #15 on Angularious/jev-selector — test now checks the endpoint path instead of an upstream display name; failure alert opens an issue; Apollo test hardened.
- Changed from first plan: agent skipped reading this harness on a "look into why it fails" task, stalled twice, and gave Jerry vague next steps. Auto-mode blocked moving `npm test` after the snapshot commit as a "CI bypass", even after Jerry said "do everything."
- Verified by: fresh `sync:catalog` run (68 APIs / 1046 endpoints), 131 tests, typecheck, `npm run build` all green in a scratch clone.
- Learned: (a) investigations are not exempt from the router; (b) hand-offs must be click-by-click recipes → [promoted: new `sops/handoff-to-jerry.md` linked from AGENTS.md, Jerry said "always"; the one-line addition to `your-eng-profile.md` was blocked by auto-mode, still pending Jerry]; (c) live-data tests should assert stable fields (paths), not upstream display names → [candidate]; (d) edits that loosen a CI gate get blocked by the harness — tell Jerry up front and give him paste-ready steps → [logged]

## 2026-09-24 — securedajob close-out (web app) · impact: low
- Built: GitHub repo (dutherightthing, private), README rewrite, 9 fresh fixtures, registrable-domain fix (open.spotify.com → spotify.com). Merged + pushed `064cb0e`.
- Changed from first plan: 3 of 7 new real links failed (Spotify, Notion, Samsara) and a passing one leaked "Head of Product Support" into PM peers. Documented as next session's first work instead of relaxing the fixtures.
- Verified by: live run of the 9 new/refreshed rows; 16 offline checks; gitleaks over the full history before the first push.
- Learned: refreshing fixtures with fresh real links is the cheapest bug-finder we have; expect ~40% of new links to expose something. For a second GitHub account, use a repo-local credential helper rather than switching the global gh account. → [logged]

## 2026-09-24 — securedajob speed + review (web app) · impact: med
- Built: overlapped the LLM + free web search with the paid DB search/roster; per-host "free read misses location" memory; 5 review fixes. Merged `bb020aa`.
- Changed from first plan: timing logs showed 4 serial fallbacks (company lookup, roster, LLM, web) of ~3s each, so overlapping them beat any single-step optimisation. The review caught an unconditional `await` on the speculative LLM call, which let a slow provider stall full answers.
- Verified by: 27/27 live suite on the local-testing key; large links 14–34s → 4–11s.
- Learned: measure per-stage timings before optimising. And when a call starts speculatively, only await it on the path that needs it, billing its ceiling otherwise. → [candidate, 1st]

## 2026-09-24 — securedajob TinyFish build + cheapest LLM (web app) · impact: med
- Built: free TinyFish page reader + LinkedIn web fallback + LLM title mapping (branch `1c-tinyfish`, 247c5b4).
- Changed from first plan: Orthogonal HOLDS $5 before every OpenRouter call, so a $2.71 balance 402'd a $0.0006 call; added Baseten GLM-4.7 (flat $0.005) as the fallback. The cheap DeepSeek/GLM OpenRouter aliases spent all their tokens thinking and never answered.
- Verified by: live regress batches 1 (9 rows) PASS incl. Cloudflare 0→4 security peers; 16 offline checks. Spent ~$1.00 vs a $0.15 estimate — debugging reruns.
- Learned (Jerry, 2026-09-24): my one-off probes must go through the Orthogonal MCP, not the app's prod key in `.env.local`; local end-to-end runs get their own dev key. → [logged]
- Learned: pick "~…-latest" OpenRouter aliases for no-upkeep models, and check Orthogonal's pre-auth hold, not just the price. Debug reruns cost money too, so quote them in the estimate. → [candidate, 1st]

## 2026-09-24 — securedajob TinyFish + Riveter check (web app) · impact: med
- Built: nothing yet. Made 2 free live calls through the Orthogonal MCP: TinyFish Fetch on Meta + SharkNinja job links, and TinyFish Search for "Cloudflare security engineer site:linkedin.com/in".
- Changed from first plan: TinyFish Fetch ($0) read Meta's title, which currently needs a $0.025 scrapegraph call, but not its location. TinyFish Search ($0) returned 10 real Cloudflare security staff with LinkedIn URLs, but mixed in India profiles. Riveter bills $0.025 per cell, and its scrape costs $0.209, so it is too expensive for a live lookup.
- Verified by: `payment: null` on both TinyFish calls; Riveter prices read from get_details.
- Learned: TinyFish Search with site:linkedin.com/in is a free people fallback when paid people-search comes back thin. Country must still be checked per result. → [candidate, 1st]

## 2026-09-23 — canvas-vault conversion fidelity (script) · impact: med
- Built: nothing new. Checked whether the econometrics markdown is safe to teach from (RMSE, MAPE, confidence intervals).
- Changed from first plan: slide charts and formula screenshots did not come through; the notes point at image files that were never saved. A PDF formula was split across lines, while the same interval written out in prose survived.
- Verified by: reading `Forecasting.md`, `Lecture Note 6`, and `Review Question.md` in `vault/2026Fall-ECN3620-01`.
- Learned: markitdown text is usable for definitions and typed numbers, and not safe as a guarantee when the source put the formula in a picture. → [logged]

## 2026-09-23 — canvas-vault first live sync (script) · impact: med
- Built: first real Babson Canvas pull — 3 current courses, 175 markdown notes in `vault/`.
- Changed from first plan: a student token gets 403 on the course Files tab; the files are on modules and linked from pages. "Active" enrollment also includes years of trainings, so only courses whose dates include today are synced.
- Verified by: live `sync.py` (175 notes, 0 conversion failures) and a rerun that updated 0 files.
- Learned: Babson student tokens must follow module items and page file links; `markitdown` without the pdf/pptx/docx extras records a failed conversion as success and then skips it forever. → [logged]

## 2026-09-22 — Jev selector gap closure (public web app) · impact: high
- Built: scheduled daily catalog refresh, complete per-provider endpoint browser, five recorded-response route tests, and four deterministic desktop/mobile Playwright screenshot checks.
- Changed from first plan: visual QA no longer depends on an interactive browser connector; Playwright uses installed Chrome, reduced motion, intercepted search responses, and checked-in baselines without spending Jev credits.
- Verified by: 16 unit/integration tests, four Chrome visual tests, lint, production build, zero npm vulnerabilities, and a no-xfront catalog sync preserving curated assets across 65 APIs.
- Learned: animated product surfaces need deterministic reduced-motion fixtures for visual regression, and catalog automation must preserve curated local assets when upstream design repositories are unavailable. → [logged]

## 2026-09-22 — Jev selector pre-finish review (public web app) · impact: high
- Built: reviewed the full uncommitted selector diff; added layered request/cost controls, strict Jev ID validation, controlled timeout handling, responsive physics bodies, logo-file checks, and password-change session revocation.
- Changed from first plan: prompt length and schema validation were treated as abuse protection, but neither limits paid request volume; no-match traffic also bypassed throttling while writing logs.
- Verified by: 11 tests, lint, Next.js production build, zero production dependency vulnerabilities, clean Gitleaks scan, and a direct Neon test proving requests 1–30 pass and 31 is blocked without calling Jev.
- Learned: paid judgment endpoints need layered controls before launch, and model-selected IDs must be resolved strictly against the supplied candidate set. Full reference: `references/past-projects/jev-selector.md`. → [logged]

## 2026-09-21 — orth-moneymaker: confetti, theme, and user-editable segments (web app) · impact: med
- Built: canvas-confetti CDN burst on the result reveal, a CSS-variable dark/light theme toggle, and full segment customization (rename up to 4 words + icon picker from a 16-item library), both synced live to the wheel and the result card.
- Changed from first plan: the wheel's radial text layout previously sized itself by measuring the actual label strings; once labels became user-editable and unbounded, switched to a fixed `LABEL_MAX_WIDTH` constant driving the layout instead, with per-label overflow detection triggering a smaller font + two-line wrap — so a long custom name can never distort the wheel's geometry, only wrap within its own space.
- Verified by: browser automation — typed a 5-word name, confirmed it capped at 4 and wrapped to two lines without touching the hub/logo; swapped icon + theme and confirmed the wheel and result card updated live; confirmed the confetti canvas appears in the DOM right after a real spin (screenshot timing made this hard to catch by eye, so verified via DOM check instead).
- Learned: 3rd layout-math issue in this one wheel (radial anchor direction, flex-order flip, now unbounded content) — same fix shape each time: drive the layout from a fixed budget, not from measuring whatever content happens to be present, whenever that content can later change size. → [candidate, recurred 3rd time in-project; Jerry declined to promote this round — needs a second *project* to confirm per the gate]

## 2026-09-21 — securedajob: code audit found an empty filter searching the planet (web app) · impact: high
- Built: a country floor on every contact search (an unreadable location was parsing to an EMPTY filter, which the provider reads as "worldwide" — the exact input set that returned Glasgow/Dubai/Vienna for a New York req); free exact company domains read off Ashby's own board page; a conglomerate rule that treats same-label ccTLD siblings as one brand's country sites; a 2 MB cap on the free HTML readers.
- Changed from first plan: I went in to audit and expected to find ranking problems. Four of the five real bugs were upstream of ranking — a parser returning empty, a domain we were paying $0.02 to guess when the page stated it for free, one question asked twice and answered by two different rules, and an enrichment record filed under a foreign legal entity. Ranking was fine.
- Verified by: live end-to-end on the four links previously reported broken — the LinkedIn one now returns 12 named people in 4.5s, all in-state; remote-US Goldman 10/10 in the US; London Stripe 14/14 in London; Lightfield 54 people with the CEO first. Screenshot + 9 green test groups.
- Learned: before adding a vendor to fill a data gap, check whether the page already states the thing — Ashby publishes the company's own website on its board, which replaced a $0.02 paid lookup AND removed the ambiguity prompt entirely. → [candidate: "the cheapest enrichment is a field the source already publishes; read the page before buying the answer"]
- Learned: an absent filter is not a neutral filter. A location parser returning `[]` looked like "no constraint" and behaved like "every country on earth". → [candidate: "when a filter can be empty, decide explicitly what empty MEANS — a floor or a fail — never let it default to unbounded"]

## 2026-09-21 — orth-moneymaker: anchored radial layout to the wrong end (web app) · impact: med
- Built: the wheel's logo+text pair now anchors to the rim (`dialRadius - logoSize - margin`) and works inward from there, instead of anchoring to the hub (`hubRadius + fixedGap`) and working outward.
- Changed from first plan: anchoring from the hub produced a small ABSOLUTE radius that doesn't grow when the wheel renders bigger (up to 460px on a wide viewport) — the whole outer ring sat empty and everything looked "bunched near the center" exactly as Jerry described from his screenshot, even though at the narrow viewport I'd tested it looked fine.
- Verified by: my own screenshot before shipping was at a viewport where the bug wasn't visible; Jerry's screenshot at a different (larger) size exposed it immediately.
- Learned: this is the SECOND layout-math bug in the same feature that only showed up outside the one viewport I happened to test (first was the flex-order flip bug two rounds ago) — a fixed-radius calculation needs to be checked against both the smallest AND largest values the container can actually render at, not just whatever the preview happened to be sized to. → [candidate, now recurred twice in-project: "for a responsive radial/transform layout, test at min AND max container size before shipping, not one arbitrary viewport"]

## 2026-09-21 — orth-moneymaker: found Simple Icons after two failed logo attempts (web app) · impact: med
- Built: swapped a hand-drawn Shopify bag glyph (my own approximation, built after a Wikimedia wordmark-recolor attempt also failed) for the real official glyph from Simple Icons (`cdn.jsdelivr.net/npm/simple-icons/icons/<slug>.svg`) — single-path, official shape, exactly the icon-only asset the badge needed.
- Changed from first plan: I went straight to Wikimedia Commons for brand logos and, when that gave a wordmark instead of an icon, hand-extracted/recolored paths or redrew shapes myself rather than checking whether a purpose-built icon library already solved this. Jerry called this out directly ("you have access to context dev and everything, why can't you literally pull the logo").
- Verified by: browser screenshot showing a correct, recognizable bag icon in place of the previous blob.
- Learned: for any well-known brand/app icon, check Simple Icons (or an equivalent purpose-built icon library) before falling back to extracting or hand-drawing from a full logo/wordmark — it exists specifically to avoid this class of mistake. → [candidate: "check for a purpose-built asset library before hand-building icon art" — this failed twice on the same logo before the fix, watch for recurrence on a different project]

## 2026-09-21 — orth-moneymaker: radial layout + logo extraction bugs (web app) · impact: med
- Built: independent per-element radial positioning (translate by pixel radius, not shared flex order) for the wheel's logo+text labels, so "logo always outer, text always inner" holds regardless of the 180° flip applied for left-half readability; a shared flex row conflated "fix orientation" with "which element sits closer to center," causing the logo to visually swap sides depending on wheel position.
- Also: hand-extracting one brand's logo paths from a multi-tone wordmark SVG and recoloring them solid (Shopify's bag+swoosh → solid white) collapsed into an unrecognizable blob at small size — the two-tone shading was load-bearing for legibility, not decorative. Redrew as a plain hand-built glyph (rect + arc) instead of trusting a "should be fine" recolor.
- Verified by: browser screenshots comparing before/after on both the flip bug (logo/text position stayed consistent across all 8 wedges after the fix) and the Shopify icon (visually confirmed as a recognizable bag, not a blob).
- Learned: rendering something and *looking at it* caught both bugs; reasoning about the CSS transform math in the abstract did not predict the flex-order bug correctly beforehand. → [candidate: "for any rotated/transformed layout, verify visually before trusting the geometry math"]

## 2026-09-21 — securedajob 1b: coverage, solved rather than reported (web app) · impact: high
- Built: Aviato as a second people source (whole-company roster ranked by seniority + department when title search is thin), Aviato company search as a startup-aware domain resolver, and a one-call render+extract rung for JS-only job pages.
- Changed from first plan: I had written off small companies as a provider limit. Wrong framing — it was a provider *choice*. Benchmarking the marketplace found one returning 54 people where icypeas returns 0, for $0.02.
- Verified by: all four failing links live. Lightfield 0 -> 13 (CEO, CTO, 4 co-founders, plus the recruiter who posted the req — email resolved to chris.doege@lightfield.app). metacareers 0 -> 20. Typical cost unchanged ~$0.023; only failures escalate.
- Learned: "the data doesn't exist" is a claim about one vendor, not about the world. Benchmark a second source before reporting a limit. → [candidate: "a coverage gap is a vendor result, not a fact"]

## 2026-09-21 — orth-moneymaker (web app) · impact: med
- Built: single-file static HTML/CSS/JS wheel spinner (no build step, no framework) for UGC creators — 8 real Orthogonal use cases as wheel segments, a hidden settings panel to force a specific result, spin-to-target rotation math, localStorage persistence.
- Changed from first plan: v1 shipped with a visible "🔒 Locked: X" pill above the wheel so Jerry could confirm the rig at a glance. Jerry corrected this — the site is filmed ON CAMERA and shown to the IG/TikTok audience, so anything that reveals the wheel is rigged defeats the entire point. Removed; the settings panel is now the only place state is visible.
- Verified by: browser automation — spun in Random mode, then locked a specific segment via the settings panel and spun twice more, confirming the pointer always landed exactly on the locked segment (screenshots in the project).
- Learned: for any tool meant to be shown on camera to an audience, never surface internal/admin state (rigged results, debug info, config) anywhere in the primary on-screen view — even a small badge is a giveaway. → [candidate: "on-camera tools must not leak their own control state to the viewer"]
- Learned: I skipped Step 0 (profile) verification mid-build and jumped from build straight to showing Jerry the result without a review/capture pass — this is the second time; worth watching for whether "skips capture under auto-mode" recurs. → [candidate]

## 2026-09-21 — securedajob 1b: who you should actually contact (web app) · impact: high
- Built: junior/intern reqs now search the bare function and weight seniority 4x, seasonal titles are excluded on every call, the widening call keeps the region instead of dropping location, and decade-old junior tenures rank down.
- Changed from first plan: the whole "peers in the same role" premise. Jerry's own report — interns are chasing return offers and can't refer anyone — was the fix; the bugs underneath it were secondary.
- Verified by: same failing search live. Before: ex-interns from 2013 in Glasgow/Johannesburg/Dubai. After: 9 current IB VPs in New York, campus recruiter 1st not 6th. Ramp regression unchanged, 5 test groups green.
- Learned: when results are wrong, probe the provider before redesigning the ranking — `exclude` was an undocumented filter that cost $0 and removed a whole class of bad rows. → [candidate: "ask the API what it can do before out-thinking it"]

## 2026-09-21 — securedajob 1b: the interface (web app) · impact: high
- Built: one internal desktop page over the live pipeline — paste a link or type a company, two columns of contacts, per-person email reveal. Loading/error/empty/unreadable-link states all real. Direction picked from 3 mockups built against a real API result, per du-design.
- Changed from first plan: the per-contact `why` sentence shipped last round turned out to be redundant the moment it was on screen — all 9 peers said the same thing. Became `flag`, set only on exceptions; the shared part is a column heading. The API got smaller because the design got made.
- Verified by: browser, not assertions — 18 contacts in 5.7s with "Different office" on exactly the 3 out-of-town people, two email reveals returning `agreed`, and an unreadable link opening the manual form. Screenshots in the thread.
- Decided: no Playwright for a single disposable internal page; `npm test` covers the logic and the README says plainly that the page has no automated test. Revisit when the page stops being disposable.
- Learned: build the screen to find the redundant data. Reviewing the JSON never surfaced it; seeing nine identical sentences in a column did. → [candidate: "render it before you finalize the response shape"]

## 2026-09-21 — securedajob 1b: result floor, 2 more ATS adapters, dossier facts (web app / API) · impact: high
- Built: plural+singular title variants in one call; a seniority backfill (labelled `adjacent`) for roles nobody holds yet; Ashby + SmartRecruiters adapters; tenure and a plain-English `why` on every contact. 720 lines of shipped code total.
- Changed from first plan: the "5-peer floor" was framed as a backfill problem and was mostly a **query** problem — "People Partners" found 1 person, "People Partner" found 3 more. Probing 4 query shapes for $0.04 found that; no amount of backfill design would have.
- Verified by: live runs on Ramp/Ashby (9+9, New York), Equinox/SmartRecruiters (10+10), Stripe manual (2→10 direct), and a fabricated req proving the adjacent path (0 direct → 10 heads/VPs in SF). `npm test` caught 2 bugs in code written the same hour.
- Learned: know which fields are load-bearing. A company-data provider with no record for Ramp was failing whole requests, but the only field the search needs is the domain, which a free source already had. It was a gate when it should have been enrichment. → [candidate: "ask of every dependency — if this returns nothing, what actually stops working?"]
- Decided: Oracle Recruiting Cloud stays unsupported on purpose — its free API returns no company name at all, so there is nothing to resolve. Documented in code rather than left as a mystery gap.

## 2026-09-21 — securedajob 1b: coverage, title precision, shared spend cap (web app / API) · impact: high
- Built: `/api/resolve` now takes `{company, title, location}` as well as `{jobUrl}`; `normalizeTitle` stops truncating at a bare rank word; guard counters moved to an atomic shared store (Upstash when configured, memory otherwise), upgrading the spend cap from a racy check to a real reservation that fails closed.
- Changed from first plan: "support more job boards" was the obvious next step and was the wrong one. A manual company/role box covers *every* board for $0 and an hour, versus one adapter per platform forever. Shipped the escape hatch instead of the tenth adapter.
- Verified by: live Stripe manual-shape call returning the 2 real "People Partners" leads (the old code returned 10 unrelated directors), live Figma/Greenhouse link at 10+10 in 3.5s, `npm test` green incl. a new fail-closed check.
- Learned: a 200 with good-looking results is not a correct result — reading the live payload closely found a bullet separator leaking `"CA • New York"` into the API as a city name; one character fixed it and moved that req 8 → 10 recruiters. → [candidate: "read the live output, not the status code" — pairs with the existing "probe the API with real money" candidate]
- Learned: precision and a minimum-results target can conflict, and the target is usually the thing that's wrong. A precise title returns 2 excellent peers where a vague one returned 10 useless ones; padding the list back to 5 is a product decision, not a bug fix. → [logged]

## 2026-09-22 — securedajob thin-results fill (web app / API) · impact: med · state: logged
- Built: short lists fill from Aviato's full roster (function- and country-filtered), expired Workday reqs detected free, staffing/RPO recruiters dropped. Verified by: full live suite 20 pass / 1 ask (by design) / 1 expired, 0 fail; Ashby UK 1 → 5 people.
- Learned (hard fact): Orthogonal pagination — page 2+ with `parentRequestId` bills $0; Aviato pages are 1-indexed and "0" aliases page 1. Read the endpoint's own billing notes before assuming each page costs. → [logged]
- Learned (hard fact): abstractapi headcount is unusable for sizing (Figma "100001+", SevenRooms 128). Don't gate logic on it. → [logged]
- Learned: literal-title search misses compound titles — icypeas has 23 Cloudflare security staff; "AI Security Research & Red Team Engineer" finds 0. Title→function mapping is the next real gap. → [candidate]

## 2026-09-23 — securedajob agency filter + merge (web app / API) · impact: medium
- Jerry's call: keep staffing/RPO recruiters (outsourced recruiters often own the req; a firm-name list needs upkeep). Removed the list; only titles that say contract/contractor/freelance are still dropped. Merged 1b to main (local, no remote).
- Learned: before telling Jerry a feature needs a new vendor key, check Orthogonal — it proxies LLMs (OpenRouter, Baseten) on the existing key and bill. → [candidate, 1st occurrence]
- Learned: jev (TypeSafe) *selects/judges* from candidates, it does not *generate*. Fits "rank these real titles against the posting", not "invent titles". → [logged; also in project memory jev-is-typesafe]

## 2026-09-22 — securedajob relevance floor (web app / API) · impact: high
- Built Jerry's hard rules: same stated country, nobody below the bar (interns/contractors/support/SDR/frontline), peers = function + its managers, small-co mode <200. Provider failures now fall through to the next source / LinkedIn, never an error.
- Live run found 3 bugs no offline test could: akta's own record files Roku under 6-inc.com; a board renamed itself "SevenRooms, a DoorDash company"; "Engineering Manager" matched a Mechanical Engineer req.
- Learned (Jerry, standing): on any gap, search Orthogonal + our history for cheap fallbacks and present options with trade-offs — never report "provider down". → [promoted to project memory: fallbacks-not-failures]
- Learned: a free directory can be confidently wrong about *itself* (akta/Roku). Two free sources that disagree are a signal, not noise — measure disagreement across the fixture list before trusting either. → [candidate, 1st occurrence]
- Miss: estimated the live run at ~$0.50; with re-runs after fixes it was ~$1. Quote re-run cost up front.

## 2026-09-22 — securedajob 1b pre-merge audit (web app / API) · impact: medium
- Audited the 18-commit branch at the three spots the handoff flagged. `npm test` green, tsc clean.
- Found: "Toronto, ON, CA" searched Toronto, *California* — same class as last session's Indiana bug, missed because the fix only ranked spelled-out countries above states. Fixed + 6 test cases (incl. "Dublin, CA" staying US).
- Learned: when a bug is fixed, probe its *siblings* with a 10-line script before closing (same parser, other formats) — cheaper than waiting for a live link to find it. → [candidate, 1st occurrence]
- Probed akta for $0 before touching aktaPick ("Harvey" → harvey.ai, 6.5× gap): guard holds, no change.

## 2026-09-21 — securedajob 1b contact targeting (web app / API) · impact: high
- Built: job link → 10 peers + 10 recruiters near the *job's* office, $0.04/5s per link; email split to an on-demand endpoint ($0.08/contact) because enriching all 20 up front cost $1.60.
- Changed from first plan: the review found contacts were filtered by the company's HQ city, not the job's — the job location was parsed by every adapter and then never used. Also proved live that the old query shape could not reach the 5-per-category target at all (1 useful result in 50).
- Verified by: two live job links (Salesforce/Workday, Figma/Greenhouse) returning 20/20 and 19/19 in-region contacts in ~5s, plus clean 400/404/501/502 on every failure path; offline `npm test` caught 3 bugs pre-spend.
- Learned: spending ~$0.06 on live API probes *before* designing beat reasoning from the vendor's docs — three design assumptions were wrong (filters are fuzzy not strict, addresses come back in localized scripts, multi-site reqs report "4 Locations"). → [candidate: "probe the API with real money before designing around it" — recurred within this project twice, needs a 2nd project to promote]
- Learned: a "max results" knob and a "max cost" knob are not the same — the flat-rate search call was never the cost lever, the per-contact enrichment was. → [logged]

## 2026-09-19 — du-code harness itself (meta) · impact: high
- Built the engineering harness as a sibling to `du-design`: router (AGENTS.md), FDE scoping skill, principles (universal/web/workflows/security), SOPs (definition-of-done/verification/code-review/testing/git-hygiene), toolbelt cards (spec-kit/ponytail/playwright/hermes/context7).
- Decisions (all Jerry's picks): sibling folder (not a monorepo); tiered rigor (quick vs full); Claude-Code-first but portable markdown so Hermes can use it too; verification bar = "show me it running."
- Repo roles locked in: spec-kit = plan, ponytail = don't over-build, playwright = prove it runs, hermes = runtime + learning model. Context7 added as a suggested reference tool.
- Open: install the toolbelt tools; point Hermes at AGENTS.md; decide on a global "read du-code first" rule; push to GitHub.

## 2026-09-27 — Greene & Hafer intake (pricing/scoping) [impact: medium]
- Quoting in hours ($50/h, then $100/h) broke client trust; switched to one fixed price for a defined result ($1,800, framed as founding-client discount off $3k standard).
- Check a client's existing tools first (dig MX showed Google Workspace) — a Sheet in their own account beat a custom Vercel+Supabase dashboard on cost, security story, and hours.
- Vercel Hobby is non-commercial; for client work use Pro or a free tier that allows commercial use (Cloudflare Pages).

## 2026-09-28 — securedajob "Still failing" pass [impact: medium]
- Of 6 benchmark failures, 2 were input parsing (a bare city, a country in brackets), 2 were the source having nobody, 1 was the scorer, 1 was scored by an old scorer. Only 3 needed product code.
- Before debugging a failing row: rescore the stored run with the current scorer, then check what the input parsed to, then ask the source for an unfiltered count (free). Each one ruled out a fix.
- Cache keys built from raw input need a version bump when parsing changes, or the old empty answer is served for a day.

## 2026-09-28 — securedajob: the benchmark's bar was the product question [impact: high]
- Three rounds of row-by-row fixes (14 → 17 → 19 of 30) hit a wall because the bar itself ("5 recruiters for every job") assumed something the data can't meet: small companies have no recruiters.
- Jerry reset it: min 3 / max 10, a "who's hiring" list (recruiters → function managers → founders under 100 people), and a check that the top person can be reached by email.
- Lesson: when fixes keep landing but the score stalls, stop and ask the owner whether the metric still matches what the user needs, before the next fix.

## 2026-09-29 securedajob: "empty" vs "thin" thresholds drift apart
When a minimum changes (10 → 3), grep every fallback trigger. The nearest-country fill still fired only on an EMPTY list, so a list of 1 passed through every fallback. One threshold constant (MIN_PER_CATEGORY) at every trigger.

## 2026-09-29 securedajob: a "second source" that never agrees is not a check
Apollo as email cross-check agreed 0 of 23 times; a $0.01 mailbox verifier told real from fake on the same domain. When two finders disagree, verify the one answer rather than pay for a second guess. Probe with a known-real and a made-up address on the same domain.

## 2026-09-29 securedajob: count the titles before writing the title list
"Head of Consulting/Investment Banking/Audit" matched 1, 8 and 9 people; the industry's own ranks matched hundreds to thousands. A free count per candidate title (icypeas /find-people/count) is the research method: 48 counts, $0, and each ladder in the code cites its numbers.

## 2026-09-30 morning-brief: keyless data first, and check who can see it
FRED serves any series as CSV with no account or key (`fredgraph.csv?id=<SERIES>`); test the ID with curl before speccing it, since 2 of 20 guessed IDs returned HTML, not data. Before putting internal or user-typed data (e.g. findtherightapi queries) on a public Vercel site, decide aggregate-only vs. login. Agents can't sign in for Jerry, so log-in-gated data means a manual export or a read-only endpoint.

## 2026-09-30 morning-brief: "public data" isn't always free to republish [impact: high]
FRED hosts third-party series whose owners forbid redistribution (ICE BofA spreads: "reproduction in any form is prohibited"; Indeed postings: "pre-approval required"). On a public site, read each series page's copyright note before adding it; swap for BLS/Fed public-domain series. Also: FRED's CSV tolerates some malformed IDs (UMCSENT_BROKEN still returned data), so test the failure path with a clearly fake ID.
