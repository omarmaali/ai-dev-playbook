# Reviewing AI-generated code

Reviewing model output is a different job from reviewing a colleague's PR, and treating it as the same job is how bad code gets merged with a clear conscience. This guide covers what's different, the failure modes worth hunting for by name, and how to run the pass. The mechanical version is [templates/ai-code-review-checklist.md](../templates/ai-code-review-checklist.md).

## Why it's a different job

With a colleague, surface quality carries information. Messy code marks the place where they were struggling, and that's where you slow down. You also know the author: their blind spots, their habits, whether they test their edge cases.

Model output takes that signal away. Everything is equally clean, equally confident, equally well-commented, whether it's correct or not. There is no author to know. So the reviewer's usual heuristic, "slow down where it looks uncertain", finds nothing to slow down for. You need a replacement heuristic, and the replacement is a list of known failure modes checked deliberately.

## The failure modes

These are the ones I check for by name. Each one has bitten me or someone near me.

**1. Invented APIs.** The model writes a call that looks exactly like the library's style and doesn't exist, or existed two versions ago, or exists with a different signature. It will present these with total confidence and, if challenged, sometimes double down with an invented changelog. The check is boring: any API call you don't personally recognize gets verified against the docs for the version pinned in the spec. Not against the model's memory of the docs.

**2. Requirement drift.** The diff does ninety percent of the spec and quietly substitutes something else for the rest, usually because the substitution was more common in training data. The wrong sort order, the wrong rounding, an "optimization" of the requirement itself. This is why review starts from the spec, line by line, and not from reading the diff top to bottom and nodding.

**3. Scope creep.** Files changed that the task didn't need, refactors nobody requested, renames that bloat the diff. Beyond review load, this is real risk: behaviour changes hide well inside cosmetic churn. Anything outside the task's file list is a reject by default, and it's a cheap reject because the spec's out-of-scope section already said so.

**4. Swallowed errors.** Generated code loves a broad `try/except` with a log line, or a fallback to a default that masks the failure. It reads as robustness. In production it means the first sign of trouble arrives weeks late and three components downstream. Every catch block gets one question: does this handle the error, or hide it?

**5. Over-engineering.** An abstraction with one caller, a config option nothing reads, a plugin system for a function. Models reproduce the average shape of enterprise code, and the average shape is over-built. Cut it during review; it only gets harder to remove once other code starts depending on it.

**6. Tests that can't fail.** Tests asserting the mock they just configured, tests of implementation detail that break on every refactor but never on a bug, tests whose assertion is tautological. Guide 4 covers evaluation in depth; at review time the quick version is to pick the two or three tests that matter and ask what change would make each one fail. If the answer is "none I can think of", say so in the review.

**7. Trust-boundary carelessness.** String-built SQL, unvalidated input passed inward, secrets as literals "for now", permissive CORS, weak crypto defaults. Nothing here is AI-specific; what's AI-specific is the polish. Insecure code used to look hurried more often than not. Now it arrives formatted and idiomatic with docstrings.

## Running the pass

Order matters, because attention runs out before code does.

Start from the spec, not the diff: take each acceptance check and find where the diff satisfies it. This catches drift (mode 2) while you're still fresh. Then walk the diff for surprises: anything outside the file list, anything the task didn't need (mode 3). Then correctness on what remains, with modes 1, 4 and 5 in mind. Then the trust boundaries. Tests last, because by then you know what the code claims to do.

Two working rules:

**Reject wholesale earlier than feels polite.** With a colleague you invest in the existing attempt, because a rewrite costs them days. A regeneration costs a minute. If the diff has two or more of the failure modes above, the fastest path is usually a tightened spec and a fresh generation, not fifteen review comments on a doomed attempt. Sunk cost reasoning transfers badly to code that took thirty seconds to produce.

**Use a model as a second reviewer, never the only one.** A fresh session with the diff and the spec (template: [review-a-diff.md](../templates/review-a-diff.md)) is cheap and catches real things, particularly drift against the spec, since it reads the spec without your assumptions. It shares blind spots with the generator, which is why it doesn't replace you.

## Accountability

The merge is yours. `git blame` will not say which lines a model wrote, and nothing downstream cares. On-call doesn't care. The postmortem doesn't care. It's the same weight senior engineers have always carried for code they approved, and it's the reason guide 1 treats the explanation bar as a merge condition.
