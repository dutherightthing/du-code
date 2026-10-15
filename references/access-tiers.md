# What am I approving? (access tiers cheat sheet)

Three levels of access. Pick the **lowest one that gets the job done**. Same idea in every tool; only the setting names differ.

| Tier | Use it for | Agent can touch | Agent can't touch |
|---|---|---|---|
| **Burner** | Scraping unknown sites, downloading files, running code from the internet | The current project folder + temp; network only where allowed | Your other folders, your logins, your Chrome |
| **Workspace** (default) | Normal project work | `~/Github Repositories/<project>` | Everything outside that folder (unless you approve it) |
| **Logged-in** | Tasks that need your accounts (Canvas, LinkedIn, Gmail) | The **Agent** Chrome profile only | Your personal Chrome profile, saved passwords, payments |

## The setting in each tool

| | Burner | Workspace | Logged-in |
|---|---|---|---|
| **Claude Code** | Type `/sandbox` (shell commands can only write to the project; network goes through an allow-list) | Default mode: asks before edits/commands outside the project | Claude in Chrome, connected to the **Agent** profile only |
| **Codex** | `codex --sandbox read-only` (or `workspace-write` with network off, its default) | `sandbox_mode = "workspace-write"` (your current setting) | Same Agent Chrome profile; **never** `danger-full-access` |
| **Cursor** (Gemini, Grok, any model) | Don't. Do burner jobs in Claude or Codex | Open only the one project folder; keep "Auto-run" off for terminal commands | Not set up |
| **Heavy option** | Docker Desktop: start it first, then run the job inside a container. Only for truly sketchy downloads | — | — |

## When a tool asks permission, what am I saying yes to?

- **"Allow edit / write file"** inside the project → usually fine. Outside the project → ask why first.
- **"Run command"** → read the command. Stop and ask if you see: `rm`, `sudo`, `curl … | sh`, `chmod`, `git push --force`, anything touching `~/.ssh`, `.env` or keys.
- **"Allow network / fetch domain"** → fine for docs and public sites. Say no for unknown download links.
- **"Always allow" / "Don't ask again" / `yolo` / `bypassPermissions` / `danger-full-access`** → that turns off every future check. Only use it in Burner, never with Logged-in.
- **Anything with money** (paid API, purchase, Orthogonal call) → the agent must tell you the price first. No price shown, no yes.
- **"Sign in" / "enter password or token"** → you type it yourself. Agents never handle your credentials.

## One-time setup (you do this)

1. Chrome → profile icon → **Add** → name it **Agent**. Log into only what agents need (Canvas, LinkedIn). No saved cards or passwords.
2. Install the Claude in Chrome extension in the **Agent** profile only.
