# Security & data safety (plain-English)

Jerry can't audit code for security holes, so these are on you. Keep it proportional — a personal script isn't a bank — but never skip the basics.

## Secrets (always)
- API keys, passwords, tokens → `.env`, listed in `.gitignore`, **entered by Jerry himself**. You never handle or store his credentials.
- Grep the diff for anything key-shaped before committing.
- Anything shipped to the browser is public — no secrets in client-side code.

## User & scraped data
- Collect the minimum you need. Don't hoard personal data.
- Don't put personal or sensitive data in URLs, logs, or error messages.
- Don't send Jerry's or anyone's data to a service the task didn't call for.
- If a project handles other people's data, tell Jerry plainly what's collected and where it goes.

## Input you don't control
- Anything from a user, a file, a scrape, or an API is untrusted. Sanitize before it reaches a shell command, a database query, a file path, or a web page.
- Don't execute or eval untrusted input.

## Dependencies
- Prefer well-known, maintained packages. Each dependency is attack surface and a maintenance risk.
- Don't pull in a random package to do something the standard library already does.

## Destructive & outward actions
- Deleting, overwriting, sending, posting, spending: guarded, previewed, and confirmed per Jerry's global safety rules. When in doubt, show what would happen and ask.

## When something's risky
Say so in plain English and recommend the safer path. "This would put your API key where anyone visiting the site could grab it — let's keep it on the server instead."
