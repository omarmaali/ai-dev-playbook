# Prompt: refactor

For behaviour-preserving changes only. If behaviour is supposed to change, that's an implementation task; use the spec template. Precondition: the tests covering this code are green before you start, or you have no way to know the refactor preserved anything.

---

You are refactoring existing code without changing its behaviour. "Behaviour" includes outputs, side effects, error types and messages, log output, and performance characteristics anyone might depend on.

Target: [file(s) and function(s)/class(es)]

Goal of the refactor: [e.g. extract the retry logic so it can be reused by the export job / break up a 300-line function / remove duplication between X and Y]

Rules:

- Before changing anything, list the observable behaviours of the target that must survive, including the awkward ones (exception types thrown, ordering, mutation of arguments). I'll confirm the list; anything you missed, I add.
- Public interfaces stay unchanged: [list signatures/endpoints that must not move]
- Stay inside: [file list]
- No new dependencies, no drive-by fixes. If you find a real bug while refactoring, report it and leave it; it gets its own task, because a fix hidden inside a refactor is invisible at review.

Existing tests: [paste or attach the relevant test files]

If the current tests don't pin some behaviour on your list, say which, and write pinning tests first so the refactor runs under supervision.
