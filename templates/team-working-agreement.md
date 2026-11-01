# AI tools: team working agreement

One page, on purpose. Fill in the brackets, delete what doesn't apply, get it reviewed (counsel sees the tools table and data rules), and put a name and a date at the bottom. An agreement nobody can recite doesn't exist.

---

**Team:** [team]  ·  **Owner of this document:** [name]  ·  **Next review:** [date, 3 months out]

## Approved tools

| Tool | Tier / agreement | Approved for | May run without asking |
|---|---|---|---|
| [e.g. Agent X] | [Enterprise agreement, no-training, SSO] | [company code and internal docs] | [read the repo, run the test suite; package installs and git push need a confirmation; never a push to main] |
| [e.g. Chatbot Y free tier] | [consumer terms] | [public information only] | [nothing; no repo access] |

Anything not in this table isn't approved. To add a tool, ask [owner]; the answer includes reading the data terms and the tool's execution defaults, not just the price.

## Data rules

Never goes into any AI tool, at any tier: credentials and keys (including expired), customer or personal data, unreleased security material (vulns, pentest findings, incident details), anything under a third party's NDA.

Company source code and internal documents: approved-tier tools only, per the table.

Repo-context tools index what they can see. `.env`, dumps, and credential stores are excluded via [tool's ignore mechanism], and that exclusion file is in the repo.

**Incident rule: a pasted secret is a rotation, not a judgement call.** Rotate immediately, post in [channel], no blame. The report is the good behaviour.

## How we work

- Tasks run through the loop: spec → generate → review → test. Specs use [templates/task-spec.md](task-spec.md) and get committed with the work.
- Sanctioned task classes right now: [e.g. tests for existing code, refactors under green tests, scaffolding]. This list grows at retro, not ad hoc.
- AI-assisted code goes through the normal PR process, reviewed with [the checklist](ai-code-review-checklist.md). Same bar as human code. The author must be able to explain any line on request; "the model wrote it" isn't an explanation.
- Whoever merges owns the merge. No exceptions by seniority, in either direction.
- Tests: the model may draft them, the task owner decides what gets asserted. The model may not change an existing test to make it pass; a red test gets reported and the task owner decides.
- This agreement, or a trimmed copy of it, is committed as the assistant's project instruction file ([CLAUDE.md / AGENTS.md / .cursorrules]) so every session starts with it.
- New dependencies proposed by a model get the same vetting as any other new dependency.

## Cadence

Weekly [30 min] retro: what did the model get wrong this week. Artifacts from it get saved to [location in repo].

---

*Agreed by the team on [date]. Changes via PR to this file.*
