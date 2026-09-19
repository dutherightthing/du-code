# Code review — self-review before showing Jerry

Run this pass on your own diff before you say a full-tier task is done. Jerry can't catch these, so you have to. In Claude Code you can also run `/code-review` on the diff; ponytail's `/ponytail-review` covers the over-building axis (see [`../toolbelt/ponytail.md`](../toolbelt/ponytail.md)).

## Review in this order (most important first)

### 1. Correctness — does it actually do the spec?
- Re-read `spec.md`. Does the code do that, including the edge cases you discussed?
- Off-by-one, wrong variable, inverted condition, wrong units, timezone/date bugs.
- What happens on empty / null / huge / malformed input?

### 2. Safety — can it hurt Jerry?
- Anything that deletes, overwrites, spends, sends, or posts — is it guarded and confirmed?
- Any secret hardcoded or about to be committed? (grep the diff for keys/tokens/passwords.)
- Any user input that reaches a shell, a query, a file path, or a URL unsanitized?

### 3. Failure handling
- Every external call (API, file, network) can fail — is it caught with a clear message?
- Does it fail *loud and safe*, or silently return something wrong?

### 4. Simplicity (ponytail axis)
- Did you write something that already exists (stdlib, a dep already installed, a one-liner)?
- Any speculative feature, config, or abstraction the spec didn't ask for? Delete it.
- Could a junior understand this in a year? If not, simplify or comment the *why*.

### 5. Fit with the rest of the project
- Matches the existing style, naming, and structure of the surrounding code.
- No copy-pasted block that should be one function.

## Output of the review
Fix what you find. Then tell Jerry, in plain English, the one or two things you were most careful about and anything you're still unsure of. Honesty about uncertainty > false confidence.
