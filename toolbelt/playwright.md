# Playwright — the verification layer

**Repo:** https://github.com/microsoft/playwright · Browser automation & end-to-end testing (Chromium/Firefox/WebKit) from Node or Python.

## Why it's core to *this* harness
Jerry can't read code, so "show me it running" is the bar. Playwright is how an agent drives a real browser to **prove a website/app works** and hand Jerry a screenshot as evidence — and leaves a re-runnable check so he can prove it again later.

## Use it for
- **Verification** (see [`../sops/verification.md`](../sops/verification.md)): load the page, click the real flow (submit the form, load the data), screenshot each key state.
- **E2E smoke tests** on full-tier web projects: one test covering the core user journey. Doubles as verification.
- Catching "looks done but the button does nothing" before Jerry ever sees it.

## When NOT to use it
- Non-web projects (scripts, agents) — verify those by running them and showing output instead.
- Pixel-perfect visual judgment — that's `du-design`'s taste pass. Playwright confirms it *functions*; du-design confirms it *looks right*.

## How to use
- **In Claude Code sessions:** the built-in browser pane / browser tools already let you load a page and screenshot it — use that for quick verification without any install.
- **For persistent E2E tests in a project:** `npm init playwright@latest` (or `pip install playwright && playwright install`), write a spec covering the core flow, and leave the run command in the README so Jerry can re-run it.

## Pattern for a verification screenshot
Load → wait for the key element → interact → assert the result is visible → screenshot. Show Jerry the before/after of the main action.
