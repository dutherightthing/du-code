# Workflow, script & agent principles

For automations, scripts, bots, scrapers, scheduled jobs, and agents — the bulk of Jerry's projects.

## Design for a non-coder operator
- Jerry runs these, so make them **easy to run and hard to misuse.** One clear command, obvious inputs, readable output.
- Print what's happening in plain English as it runs ("Fetched 42 rows… writing to results.csv"), so he can tell it's working.
- Fail with a message a human understands, plus what to do about it.

## Robustness
- **Assume inputs are messy** — empty, malformed, duplicated, rate-limited. Handle it.
- **Retries & rate limits** for anything hitting an external API. Back off, don't hammer.
- **Idempotent where possible** — running it twice shouldn't double-charge, double-send, or corrupt data.
- **Never lose partial progress** on a long job — write as you go or checkpoint, so a crash at row 900 doesn't waste the first 899.

## Money, messages, and data — the danger zone
- Anything that **sends** (email, DM, SMS), **spends** (paid API, purchase), or **deletes**: default to a dry-run / preview that *shows* what it would do, and require explicit confirmation to actually do it.
- Never auto-execute financial actions. Show Jerry; he pulls the trigger.

## Scheduled / long-running
- Say clearly: how it's triggered, how Jerry checks it's alive, how he stops it, what it costs to run.
- Log runs somewhere he can look later.

## Secrets & data
- Keys in `.env`, entered by Jerry. Scraped/collected personal data handled per [`security.md`](security.md).

## Verify (see [`../sops/verification.md`](../sops/verification.md))
- Run it end-to-end on realistic input, show the real output, and show what it does on a bad input.
