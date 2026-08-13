# Team onboarding

How to get a team from "some people quietly use ChatGPT" to a shared, reviewable practice. That starting point is worth naming: if you think your team isn't using AI tools yet, what you actually have is usage without rules. The rollout below assumes it.

I've run enablement in enterprise settings for years, and the pattern behind most failed tool adoptions isn't the tool. It's sequencing: mandating before piloting, measuring before defining, training on features instead of judgement. The plan here is deliberately boring in the right places.

## Before anyone starts: the preconditions

Two things exist before the first sanctioned task, both from [guide 3](03-security-and-ip.md):

1. An approved tool, by name and tier, that people can actually access. If the sanctioned tool is worse than the unsanctioned one, or takes three weeks of tickets to get, the unsanctioned one wins and you've spent your credibility on a rule nobody follows.
2. The [working agreement](../templates/team-working-agreement.md), adapted and visible. One page: tools, data rules, review rules, incident rule.

That's it for preconditions. Waiting for a perfect policy is one of the ways rollouts stall; the agreement has a review date on it precisely so it can start imperfect.

## The first week, for one developer

Start narrow. Three task classes carry low risk and teach the loop fast:

- **Tests for existing code.** The code already works, so the blast radius is a bad test, and writing tests for code you didn't write is exactly the review skill the loop needs anyway.
- **Small refactors under existing tests.** Green before, green after; the suite does the supervision.
- **Scaffolding.** New endpoints, migrations, boilerplate along patterns the codebase already has. Easy to review because the reference implementation sits one file over.

The exercise: pick one real task from the current sprint in one of those classes, write the spec with [templates/task-spec.md](../templates/task-spec.md), run the full loop including the checklist review, and merge through the normal PR process. One real task beats a sandbox tutorial, because the friction you hit is the friction that's real.

Then do the one thing I'd call non-negotiable: **pair-review the first few AI diffs.** Two people, one screen, walking the checklist together. Partly training, mostly calibration: it's where the team develops a shared answer to "how carefully do we actually read this stuff", and that answer is the real control in the whole system.

## Rolling out to the team

**Pilot with two to four volunteers for about four weeks.** Volunteers, because early friction reported by a skeptic reads as "the tool is bad", and the same friction reported by a volunteer reads as data.

**Hold a weekly half-hour retro with one agenda item: what did the model get wrong this week.** The framing is deliberate: reporting failures becomes the norm rather than a confession, and the team builds its own catalogue of model mistakes faster than any document could teach it. It also keeps expectations calibrated while the marketing around these tools runs hot. The best sessions produce artifacts: a spec that turned out ambiguous, a diff that hid a behaviour change, a test that couldn't fail. Save them.

**Keep team prompts and specs in the repo.** When someone writes a spec or prompt that works well, it gets committed next to the templates, versioned and reviewable like everything else. Institutional knowledge in a shared drive is where institutional knowledge goes to die.

**Expand when the pilot's artifacts can teach.** The signal that you're ready isn't a metric threshold; it's that the pilot group can hand the next group a stack of real examples from the retros. If four weeks produced nothing worth showing, the pilot isn't done.

## What to expect

Individual throughput dips before it rises, because the loop front-loads work into specs and review that used to be skipped or implicit. Plan for it, say it out loud at the kickoff, and don't run the pilot during a crunch, where the first schedule pressure will squeeze out exactly the stages that make the thing safe.

And watch the junior/senior asymmetry, because it's the opposite of what the tools' marketing implies. Seniors get the most out of AI assistance: they write tighter specs and their review actually catches things. Juniors generate at the same speed with far less ability to judge the output, and the generated code's polish actively hampers learning; it never looks like it needs questioning. More review for junior-authored AI diffs, not less, and pair the pilot's juniors with its seniors. A junior who learns to write specs and review hostile diffs is getting a good education. A junior who learns to accept suggestions is not.

## Anti-patterns

Each of these has sunk a real rollout:

- **Mandating usage.** Produces resentment and fake compliance, and destroys your failure-reporting channel on day one.
- **Usage leaderboards.** See guide 4 on measurement; a leaderboard is the gameable metric with social pressure added.
- **Two-tier rules.** Seniors exempting themselves from the checklist while juniors follow it teaches everyone the rules are theatre. The people with the most credibility need to be the most visible about following the process.
- **Crunch-time adoption.** The loop's value is the discipline, and discipline is the first casualty of a deadline. Adopt in normal weather.
- **Tool-feature training.** A lunch-and-learn about the assistant's features teaches people to generate more. Train on specs and review instead; generating was never the hard part.
