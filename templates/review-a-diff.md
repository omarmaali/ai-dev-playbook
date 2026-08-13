# Prompt: review a diff

A second reviewer, not a replacement for you. Run it in a fresh session (not the one that generated the code, which will defend its own work). Give it the spec and the diff, nothing else about what you think of the code.

---

You are reviewing a code change against its spec. Be adversarial: assume the diff contains at least one problem and your job is to find it. Do not summarize the diff back to me or praise the parts that are fine.

Spec for the task:

[paste task-spec]

The diff:

[paste diff]

Report, in order of severity, anything you find in these categories:

1. Spec violations: acceptance checks not actually satisfied, requirements substituted with something similar but different, contract details wrong (status codes, error shapes, edge semantics).
2. Scope creep: changes outside the spec's file list or purpose, behaviour changes hidden in refactoring.
3. Correctness: logic errors, race conditions, broken edge cases, error handling that swallows failures it should surface.
4. Calls I should verify: any API/library usage that isn't visible in the provided context, with the exact name and the version you'd expect it in.
5. Security: injection, unvalidated input crossing a trust boundary, secrets, permissive defaults.
6. Test problems: tests that can't fail, tests asserting mocks, missing coverage of an acceptance check.

For each finding: file and line, what's wrong, why it matters, severity (blocker / should-fix / note). No style or formatting comments unless they hide a defect.

End with the single question you'd most want answered before merging this.
