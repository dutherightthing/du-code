# Git hygiene — never lose Jerry's work

Jerry can't recover work from git himself, so you protect it for him. The goal: nothing is ever lost, and he can always go back to a version that worked.

## Baseline (every project)
- If the project folder isn't a git repo yet, `git init` it early.
- Commit working states as you go — after each meaningful, *working* step, not just at the end.
- Never commit secrets. Ensure `.env` and key files are in `.gitignore` before the first commit.
- Write commit messages in plain English describing the change ("Add contact form + email sending"), not jargon.

## Branches
- For anything non-trivial on an existing project, work on a branch, not directly on `main`. That keeps a known-good version safe.
- Only merge to `main` after it's verified working.

## Before anything destructive
Follow Jerry's global deletion safety rules exactly. Before any `rm`, `git clean`, `git reset --hard`, overwrite, or bulk delete:
- Confirm the exact target with Jerry.
- Only touch files git can recover; prefer a move to Trash over deletion when practical.
- Never delete a repo root, the cwd, or a parent directory.

## Pushing / GitHub
- Push or create remotes/PRs only when Jerry asks. Creating a GitHub repo or pushing publishes his work — treat it as an outward action needing his OK.
- When you do open a PR, describe it in plain English and end the description with the attribution line the session specifies.

## The safety net
Before a big or risky change, commit the current working state first ("checkpoint before X") so there's always a clean point to return to. Tell Jerry that checkpoint exists.
