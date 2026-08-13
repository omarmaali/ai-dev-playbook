# Task spec

Copy this into your task tracker or a scratch file, fill it in, and paste it into the generation prompt. One page maximum; most tasks need half of one. If you can't fill in the acceptance checks, the spec isn't ready and neither is the task.

---

## Task

[One or two sentences. What changes, in plain language.]

## Context

[Why this is needed, and anything the model can't infer: the system it sits in, the convention it must follow, the incident that motivated it.]

## Contract

[Observable behaviour. Inputs, outputs, side effects. For an API: request/response shapes and status codes. For a function: signature and semantics. For a job: trigger, effect, idempotency.]

## Constraints

- Language/runtime: [e.g. Python 3.13]
- Frameworks/libraries and versions: [pin the ones that matter]
- Files that may change: [explicit list]
- Interfaces that must be used: [existing clients, helpers, patterns to follow, with file paths]
- New dependencies: [usually "none without asking"]

## Out of scope

[What the model must not touch or "improve". Be blunt; this list is what makes drift a mechanical reject at review.]

## Acceptance checks

[2 to 6 statements you could turn into tests without further discussion.]

1. [ ...]
2. [ ...]
3. [ ...]

---

## Filled example

**Task:** Add per-client rate limiting to `POST /api/v1/quotes`.

**Context:** One integration partner sends bursts that starve other clients. We already identify clients by the `X-Client-Id` header (validated in `middleware/auth.py`). No other endpoint gets limiting in this task.

**Contract:** Requests over the limit get `429` with body `{"error": "rate_limited", "retry_after_seconds": <int>}` and a `Retry-After` header. Limit: 60 requests per rolling 60 seconds per client id. Requests under the limit are unaffected.

**Constraints:** Python 3.13, FastAPI 0.115. May change: `middleware/ratelimit.py` (new), `main.py` (wiring only), `tests/test_ratelimit.py` (new). Must use the existing Redis client from `infra/redis.py`. No new dependencies.

**Out of scope:** other endpoints, auth logic, Redis configuration, retry behaviour of our own outbound clients, log format changes.

**Acceptance checks:**

1. 61st request from one client id within 60s gets 429 with the documented body and header; the 60th gets through.
2. Two different client ids each get their own 60; one client bursting doesn't affect the other's budget.
3. If Redis is unreachable, requests pass through unlimited and one warning is logged per connection failure, not per request.
4. Existing `POST /api/v1/quotes` tests still pass unchanged.
