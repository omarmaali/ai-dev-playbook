# Evaluating AI-generated code

Two questions in this guide: how to judge a specific piece of generated code, and what to measure at team level once AI is in the workflow. They get conflated in most adoption conversations, and they shouldn't be, because one is engineering and the other is mostly politics wearing engineering's clothes.

## The bar

Same bar as human code. One bar per codebase.

A lower bar for generated code is an obvious mistake, but the common failure is subtler: an informal *higher* bar, where reviewers nitpick generated diffs to prove a point while waving through a colleague's work. The output of that isn't quality. It's developers hiding which code was generated, which kills the review discipline you actually need.

What changes isn't the height of the bar, it's where you point the flashlight; the places worth deliberate suspicion are catalogued in [guide 2](02-reviewing-ai-code.md).

## Red flags

Things in a generated diff that should make you slow down. None proves a defect. Each one has, in my experience, better-than-even odds of sitting near one.

- **Handling the spec never asked for.** A parameter validated that can't be invalid here, a retry around a pure function, an edge case for a state the system can't reach. It means the model was pattern-matching some other program, and the question is what else it imported from that program.
- **Comments narrating the code.** `# increment the counter`. Beyond noise, it's a provenance tell, and the comment density is usually uniform across the file, which no tired human achieves.
- **Nothing calls it.** Helpers exported and never used, config options nothing reads. Generated scaffolding for an imagined future. Dead on arrival, but it still costs review and maintenance.
- **The catch-all.** `except Exception: pass`, or its politer sibling that logs and continues. Covered in guide 2; flagged again here because it's the single most common serious defect I see in generated diffs.
- **Tests that mirror the mock.** The test configures a mock to return X, calls the code, asserts X. Green forever, tests nothing except the mocking library.
- **Everything passes first try.** A whole generated suite green on its first run should trigger the question guide 1 ends on: can any of these actually fail? Fresh tests against real code usually hit at least some friction.

## Coverage, and what it's worth

Coverage numbers are the metric most likely to be quoted and least likely to mean anything, because generated tests are spectacularly good at coverage. Executing a line is cheap; asserting something that matters about it is the expensive part, and the percentage can't tell the difference.

If a claim about test quality needs to hold weight, the honest measure is mutation-shaped: break the behaviour deliberately and confirm the suite notices. I keep this concrete because I got burned concretely; the mutation pass described in the [README](../README.md#where-this-comes-from) found four guardrails in my own repos whose tests couldn't fail, and every one of them contributed to a coverage number that looked fine. Full mutation testing on everything is expensive. Reserve the rigor for code where failure is expensive: guardrails, money paths, permission checks, data deletion. For those, "delete the check, run the suite, confirm red" is a ten-minute investment with an unambiguous answer.

## Measuring the team

Sooner or later someone asks for the productivity number. Two things I'd want any engineering leader to hold onto.

Don't measure AI usage as output. Lines generated, "% of code AI-written", assistant sessions per developer: all of it is trivially gameable, none of it correlates with delivery, and the moment any of it becomes a target, developers optimize for it and the number stops meaning even the little it meant. Worst case you get generated bloat celebrated as productivity, which is negative productivity with a dashboard.

Measure the things you already believed in, and watch their trend through adoption: escaped defects, rework rate (how often merged work gets reopened), review turnaround, cycle time on comparable task classes. If AI adoption is working, it shows up there, gradually. Expect the review-time trend to worsen at first; guide 5 explains why that's the plan working, not failing.

And a caveat I'd rather state than have you discover: clean attribution is not available. You will not get "AI made us 23% faster" out of trend metrics on a team that's also shipping, hiring and changing scope. Anyone who quotes a precise number for their own team got it from vibes or from a vendor deck. The trends are enough to answer "is this hurting us or helping us", and that's the only question your rollout metrics should promise to answer.
