# Prompt: write tests

For adding tests to existing code. The enumerate-first step is what stops you getting fifty shallow tests that all exercise the happy path; you choose the cases, the model does the typing.

---

You are writing tests for existing code. The goal is tests that fail when the behaviour breaks, not tests that execute lines.

Code under test: [attach file(s)]
What it's supposed to do: [contract, or paste the original spec if one exists]
Test framework and conventions: [e.g. pytest, fixtures in conftest.py, see attached example test file]

Step 1, before writing any tests: enumerate the cases as a list. Normal cases, boundaries, error paths, and any behaviour of the code that looks unintended (flag those separately; I need to decide whether they're bugs or contracts before you pin them). Wait for me to confirm and prune the list.

Step 2, write the tests, under these rules:

- Each test asserts observable behaviour: return values, raised exceptions, side effects. Not internal call patterns.
- Mock only at real boundaries (network, clock, filesystem, other services): [list what's mockable here]. Never assert that a mock returned what you configured it to return.
- No test reads ambient state: no dependence on my environment variables, my files, my database, or test execution order. Fixtures build what they need.
- For each test, add a one-line comment saying what change to the code would make it fail. If you can't write that line, drop the test.
- Deterministic: no sleeps as synchronization, no real time, no real randomness without a seed.

If any test you write passes against the current code but shouldn't (you found a bug in step 1), mark it clearly with the expected-correct assertion commented out, and list it at the end.
