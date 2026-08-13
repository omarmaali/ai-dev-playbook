# AI code review checklist

The reviewer's pass for AI-authored or AI-assisted diffs. Run it in this order; the early sections catch the problems that make the later sections moot. Reasoning behind each section is in [docs/02-reviewing-ai-code.md](../docs/02-reviewing-ai-code.md).

Two or more blockers in the first three sections? Consider rejecting wholesale and regenerating from a tightened spec instead of commenting line by line.

## Spec conformance

- [ ] Every acceptance check in the spec is satisfied by something I can point to in the diff
- [ ] No requirement was substituted with a near-neighbour (sort order, rounding, error shape, default values checked against the spec's words, not the code's comments)
- [ ] The contract details are exact: status codes, exception types, response shapes, header names

## Surprises

- [ ] No files changed outside the spec's file list
- [ ] No refactors, renames or "cleanups" that weren't requested
- [ ] No new dependencies, or each new dependency was explicitly approved and is the package I think it is (name checked, maintained, not a look-alike)
- [ ] Nothing added "for future flexibility" that the spec didn't ask for

## Correctness

- [ ] Every API call I don't personally recognize verified against the docs for the pinned version, not assumed
- [ ] Each `catch`/`except` either handles the error meaningfully or lets it propagate; nothing logs-and-continues past a failure the caller needs to know about
- [ ] Failure paths lead somewhere sane: no silent fallbacks to defaults that mask a broken dependency
- [ ] Concurrency, if touched: shared state, ordering and cancellation reasoned about, not vibes
- [ ] I can explain every line. Lines I can't explain don't merge

## Trust boundaries

- [ ] No string-built SQL/queries; parameterized everywhere
- [ ] Input crossing a trust boundary is validated on this side of it
- [ ] No secrets in code, config defaults, tests or fixtures
- [ ] Permissions/authz checks on the new path match the rest of the codebase
- [ ] Nothing weakened "temporarily": CORS, TLS verification, crypto parameters

## Tests

- [ ] Each acceptance check has a corresponding test
- [ ] For the tests that matter most, I can name a code change that would make them fail
- [ ] No test asserts a value it just configured a mock to return
- [ ] Tests build their own fixtures; none read my environment, my files or my database
- [ ] Guardrail/money/permission logic: the check was disabled or inverted at least once and the suite went red (see [docs/04](../docs/04-evaluating-ai-code.md))

## Housekeeping

- [ ] Narrating comments deleted (`# increment the counter`); remaining comments explain why, not what
- [ ] No dead code: unused helpers, unread config options, unreachable branches
- [ ] Logging on the new path matches house style and won't leak the data guide 3 says never leaves
