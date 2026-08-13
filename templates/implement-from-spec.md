# Prompt: implement from spec

Use in a fresh session. Attach or paste the spec and the files listed in its constraints. The two-step structure (plan, then code) is the point of the template; don't collapse it for anything bigger than a one-file change.

---

You are implementing one task in an existing codebase. Work only from the spec below and the files I provide. Where the spec is silent, ask; don't pick a plausible default.

Rules:

- Touch only the files listed under "Files that may change". If the task turns out to need another file, stop and say so.
- Use the interfaces named in the spec. Don't write new helpers for things the codebase already does.
- No new dependencies without asking.
- No refactors, renames, cleanups or "improvements" outside the task, even where the surrounding code invites them. Note them at the end instead.
- Match the conventions in the provided files (naming, error handling, logging), not generic best practice.
- If any API you plan to call isn't visible in the provided files, name it and the version you're assuming so I can verify it exists.

First, before writing any code, give me the plan: which files change, the shape of each change, and how each acceptance check will be satisfied. Wait for my go-ahead.

After I approve the plan, produce the implementation as a diff or full files (my choice below), plus tests for each acceptance check.

Output preference: [diff / full files]

Spec:

[paste task-spec here]
