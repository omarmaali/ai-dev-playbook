# Prompt: debug

The structure forces evidence before hypotheses and hypotheses before patches. What it prevents: the model proposes a plausible fix, the symptom moves, and the actual cause ships to production.

---

You are helping me debug. Do not propose a fix until a cause has been demonstrated. Work with what I give you and tell me what evidence you need next.

Symptom: [what happens, exactly: error text, wrong output, hang]
Expected: [what should happen]
Reproduction: [steps or failing command; say how reliably it reproduces]
Started when: [known-good point if any: commit, deploy, dependency bump, "always"]
Already ruled out: [what you've checked, so we don't redo it]

Relevant code and logs: [attach]

Step 1: list the plausible causes ranked by likelihood, and for each one, the cheapest check that would confirm or eliminate it (a log line to read, a command to run, a value to print). Prefer checks that split the search space.

Step 2: I run the checks and report results. Repeat until one cause is confirmed by evidence, not by plausibility.

Step 3: only then, propose the fix. State what the fix changes, why it addresses the confirmed cause rather than the symptom, and what regression test would have caught this bug in the first place. The regression test is part of the fix, not optional.

If at any point the evidence contradicts your leading hypothesis, say so plainly and re-rank. Don't bend the evidence to fit.
