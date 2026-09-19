# Scoping skill — the FDE grill

**Purpose:** Jerry is not a coder. He'll describe what he wants in outcomes, not specs, and he'll leave out things he doesn't know are decisions. Your job here is to pull the real project out of him, surface the hidden choices, write it down, and get a yes — *before* any code.

Think like a Forward-Deployed Engineer meeting a client: you don't take the brief at face value, you interrogate it kindly until it's buildable.

---

## How to run it

Two modes (the router already picked the tier):
- **Quick** — ask only the ⭐ questions below, confirm in one line, go.
- **Full** — work through all six areas, then write `spec.md` and play it back.

**Ask questions in small batches (2–4 at a time), in plain English, with a recommended default for each.** Never dump 20 questions at once. If Jerry says "you decide," decide and note your choice in the spec.

---

## The six areas to grill

### 1. Outcome & success ⭐
- What does "done and working" look like, concretely? Describe the moment you'd use it.
- Who is this for — just you, or other people? How many?
- ⭐ How will *we* know it worked? (the thing we'll check at the end)

### 2. Scope & anti-scope ⭐
- ⭐ What's the smallest version that's still useful? (we build that first)
- What is explicitly *out* for now? (name it so we don't gold-plate)
- Is this a throwaway or something you'll keep building on?

### 3. Inputs, data & integrations
- What does it need to read or connect to? (files, a site, an API, your email, a sheet)
- Where does the data come from and where does it go?
- Any accounts/keys involved? (Note: Jerry enters his own secrets — you never handle credentials.)

### 4. Constraints & environment ⭐
- ⭐ Where does this run and who sees it? (your laptop / a website / a schedule / other people)
- Any deadline?
- Budget sensitivity — does it call paid APIs or hosting? (matters; Jerry should know the cost)
- Does it need to keep running (a schedule, a server) or run once?

### 5. Risks & blind spots (you raise these — he won't)
Proactively surface whatever applies:
- **Data loss** — could this delete or overwrite something he cares about?
- **Money** — can it spend, trade, or send? (never automate these without explicit per-action confirmation)
- **Privacy** — does it expose personal data, or post/send anything publicly?
- **Auth** — if others use it, who's allowed in?
- **Failure** — what happens when the API is down or input is weird?

### 6. Taste & references (hand off to du-design if visual)
- Any examples of what "good" looks like here?
- If there's a UI, route the visual side to `../du-design`.

---

## Output: write `spec.md` into the project

For full-tier, write this to the **project folder** (create the folder if needed), then read it back to Jerry in plain English and wait for a yes:

```markdown
# <Project> — spec
_Date · agreed with Jerry_

## What we're building (one paragraph, plain English)

## Done looks like
- [ ] <the concrete check we'll run at the end>

## In scope (v1)
-

## Out of scope (for now)
-

## Inputs / data / integrations

## Runs where / who sees it

## Risks flagged & decisions
- <risk> → <what we decided>

## Open questions
-
```

**The rule:** if you can't fill in "Done looks like" with something checkable, you haven't scoped enough yet. Keep grilling.

---

## After the yes
Hand back to the router: Plan (task list) → Build → Verify → Review → Capture. Keep `spec.md` in the project as the source of truth; if scope changes mid-build, update it and tell Jerry what changed.
