# Verification — prove it works

Jerry can't read code, so "it should work" is worthless to him. He chose **"show me it running"** as the bar. Every deliverable ends with evidence he can see.

## The principle
> You don't get to say "done." You get to *show* done.

## How to show it, by project type

**Website / web app**
- Start it and open it in the browser (Playwright / the built-in browser — see [`../toolbelt/playwright.md`](../toolbelt/playwright.md)).
- Screenshot the working page(s). Click the key flow (submit the form, load the data) and screenshot the result.
- If it's deployed, verify the live URL too, not just localhost.

**Script / automation / workflow**
- Run it on real (or realistic) input.
- Show the actual output — the file it made, the rows it returned, the message it would send (show, don't send, unless Jerry approved sending).

**Agent / bot**
- Run one real end-to-end interaction and paste the transcript.
- Show what it does on a weird/empty input, not just the happy path.

**Scheduled / long-running thing**
- Trigger one run manually and show it completing.
- Say clearly how Jerry checks it's still alive later.

## Rules
- **Realistic input, not toy input** — if it'll process his emails, test on a real-shaped email.
- **Test the failure path too** — show what happens when the API is down or input is empty. "Handles errors" means you saw it handle one.
- **Make it re-runnable.** Leave a one-line command (in a ```bash block) or a saved Playwright check so Jerry can prove it to himself later.
- **If you couldn't verify something, say which part and why.** Don't paper over it.

## Clean up after yourself (no orphans)
Verifying means starting things (dev/preview servers, headless browsers, screenshot loops). Left running, they pile up for days — they were the random Chrome icons flashing in Jerry's Dock (found 2026-10-05: servers 6–15 days old, a Chrome stuck 7 days).
- **Stop what you start before you say "done."** Servers, preview ports, background jobs, browsers. Use `preview_stop` for servers from `preview_start`; for anything else, kill the PID you launched and confirm it's gone (`ps -p <pid>`).
- **Never launch `/Applications/Google Chrome.app` headless for screenshots.** macOS gives every launch a Dock icon. Use Playwright's bundled browser instead (no Dock icon, no `.app`): `playwright screenshot <url> out.png` — see [`../toolbelt/playwright.md`](../toolbelt/playwright.md).
- **No bare `--foreground` servers or screenshot loops without a timeout or a stop step.** If a loop could hang (4 screenshots in a row), wrap each call in `timeout 60`.
- If you leave something running on purpose (Jerry asked for it), say what, on which port, and how to stop it.

## Minimum evidence to include when you report "done"
1. What you ran (the command or the steps).
2. What happened (screenshot / output / passing test).
3. One plain-English line: "This means <the thing Jerry wanted> works."
