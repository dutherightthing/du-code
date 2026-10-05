# Handoff to Jerry — how to finish a turn so he never has to guess

Jerry is not a coder. When work reaches the point where *he* has to click, merge, paste, or approve something, the turn is not done until he can do it **without interpretation**. Vague directions ("update the workflow", "merge and re-run", "add a permission rule") are a failed handoff, even if the engineering was right.

Origin: 2026-10-05, jev-selector CI fix. Agent stalled twice and handed back jargon. Jerry: "you keep stopping beforehand and giving me vague directions I don't understand."

## 1. Don't stop early
- If the next step is clear and inside what he asked for, **do it** — don't narrate it and wait.
- "Go ahead" / "do what is needed" / "do everything" covers every step needed for the outcome he named, including opening PRs. Only stop for things that are hard to reverse or cost money (see his global rules) — and then ask **one** specific yes/no question, never a menu of options.

## 2. If something is blocked, say so FIRST
Some actions get blocked by the permission system (editing CI/test gates, writing to `main`, config files, files outside the project folder). When one is blocked:
1. Do everything else that isn't blocked.
2. Say plainly what was blocked and why, in one sentence, at the *top* of the reply.
3. Convert the blocked piece into steps Jerry can perform (section 3). Don't retry the same thing another way to get around it.

## 3. Write steps like a recipe (the format)
Numbered. One action per step. Every step names **where** (a clickable link or exact button label) and **what to do**. Anything to type or paste goes in its own code block — **the complete text, not a description of it**.

- Use real links, not "go to the repo". e.g. `https://github.com/<owner>/<repo>/pull/15`
- Name buttons exactly as they appear ("Merge pull request", then "Confirm merge").
- If he must edit a file, give the **whole finished file** to select-all-and-paste, not "move line 34 below line 46".
- Say what he'll see when it worked ("a green check appears in about 2 minutes").
- Say what to do if it didn't ("send me a screenshot of the red X").
- No unexplained jargon (CI, workflow dispatch, gate, rebase, snapshot). One short plain phrase the first time, or skip the word.
- Offer the shortcut: if a step could be done *for* him by an agent with one approval, say that too.

## 4. End-of-turn shape
1. **What's done** (one or two lines, outcome not files).
2. **What I need from you** — the numbered recipe, or "nothing, you're set."
3. **What happens next / how you'll know** — one line.

Keep the reasoning and file-level detail out unless he asks. Put it after the steps if it's worth keeping.

## 5. Scope before an investigation too
"Look into why X fails" is not exempt from the router. Read `AGENTS.md` first, then before fixing, tell Jerry in plain words: what you found, what you plan to change, which parts touch live things (production data, the live site, `main`, spending) and which don't, and what you'll need from him. Then proceed unless it crosses a line in his global rules.
