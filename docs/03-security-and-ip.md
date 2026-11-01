# Security, IP and governance

This is the guide that decides whether your AI adoption survives its first incident. It's an engineer's view, not legal advice; the point where you hand your draft policy to counsel is marked below, and you should actually do it.

The risks come in three directions, and mixing them up produces bad policy:

- **What leaves**: your data, code and secrets going into someone else's system via prompts.
- **What arrives**: generated code carrying licence problems or vulnerabilities into your codebase.
- **What's retained**: what the vendor stores, for how long, and whether it trains on it.

## What leaves

The prompt is an exfiltration channel with excellent UX. Nobody thinks of pasting a config file at a chatbot as "sending company data to a third party", and that's precisely what it is. The working rule I give teams:

**Never paste, into any AI tool at any tier:**

- Credentials of any kind: API keys, tokens, connection strings, private keys, passwords. Including "expired" ones, which have a way of not being.
- Customer data and personal data. Real names, emails, account records, message contents. If a bug needs the shape of the data, synthesize the shape.
- Unreleased security material: vulnerability details, pentest findings, incident specifics.
- Anything under someone else's NDA. Their contract didn't anticipate your tooling.

**Paste only into tools at the approved tier** (next section): proprietary source code, internal architecture, unreleased product details.

And the incident rule, which belongs verbatim in your working agreement: **a pasted secret is a rotation, not a judgement call.** Rotate it the moment it's noticed, tell the team, no blame attached. This is the same lesson ops learned about outage reporting years ago.

A channel people forget: assistants with repo-wide context upload what they index. Curating context (guide 1) isn't only about output quality. `.env` files, dumps and credential stores should be excluded from what the tool can read at all, using the tool's ignore mechanism, and that exclusion belongs in the repo where it's versioned.

## Tool tiers

Not all "AI tools" are one thing contractually, and the contract is what you're actually trusting.

| Tier | Typical terms | Suitable for |
|---|---|---|
| Consumer chat apps, free tiers | May train on inputs; retention by default | Public information only. Nothing proprietary |
| API / paid business tiers | Typically no training on inputs; defined retention | Internal code and docs, if the terms have been read |
| Enterprise agreements | Negotiated: no-training, retention limits, region, SSO/audit | The default for company code |

Two habits make the table usable. First, someone specific owns it: reads the actual data-processing terms, not the marketing page, and re-checks when the vendor's terms change, which is often. Second, tools are approved by name and tier and listed in the [working agreement](../templates/team-working-agreement.md). "We use the ChatGPT" is not a policy; which offering, under which agreement, for which data, is.

Agents add a second question to the table: not only what the tool may see, but what it may do. An agent with shell access can install packages, call the network, delete files and push commits, and the defaults differ by tool and change between versions. Record the answer per tool: what it may run without asking, what needs a human confirmation, and what is off entirely (pushing to a shared branch, touching production credentials, anything that leaves the sandbox). The [working agreement](../templates/team-working-agreement.md) has a column for it.

This is the paragraph to take to counsel, together with your data classification. It's a short conversation when you arrive with a draft table, and a very long one when you arrive after an incident.

## What arrives: licensing and provenance

Generated code is mostly unremarkable on this front: short, spec-driven output shaped by your constraints. The risk concentrates in a specific pattern, the large dropped-in block: ask a model for "a complete X" and you can receive something structurally close to a particular open-source implementation, licence not included. GPL code arriving unmarked in a proprietary codebase is the scenario your counsel loses sleep over.

Practical mitigations, in the order I'd adopt them:

1. Prefer generation against your own spec and interfaces over "write me a library". This is the same practice that produces better code anyway, which is convenient.
2. Treat any large, complete, suspiciously polished block as a provenance question. Searching a distinctive line or two takes seconds.
3. If the codebase already runs licence scanning in CI, generated code flows through it like everything else. If it doesn't, AI adoption is a good reason to start; the tooling is mature.
4. Dependencies the model added get the same scrutiny as dependencies a person added. Models suggest packages by familiarity, and familiarity in training data includes abandoned and typo-squatted packages. Verify the package exists, is maintained, and is the one you think it is.

On the ownership of output: vendor terms generally assign you whatever rights they can, but the reliable position is that your value lives in the spec, the review and the integration, all human work products. Check the terms of your specific tier anyway; this is the second item for the counsel conversation.

## Governance that people actually follow

Everything above compresses into a one-page working agreement: approved tools by name and tier, the never-paste list, allowed task classes, review rules, test ownership, the incident rule, an owner, and a review date. The template is [templates/team-working-agreement.md](../templates/team-working-agreement.md). One page is a feature; I've never seen page nine of a policy quoted in an incident channel.

A note on audit trails, because it comes up in every rollout conversation: mandatory "AI-assisted" labels on commits sound like governance and don't work. They're unverifiable, they decay into noise within a month, and they imply the review bar varies by author, which is exactly backwards. Put the enforcement where it's enforceable: every line merges through the same review, and the reviewer owns the merge. Guide 2 is the control. If leadership wants adoption metrics, ask the tools' admin consoles, not the commit log; that data is at least honest about being incomplete.

What's genuinely worth keeping: specs. They're the record of what was asked for and what the acceptance bar was, they cost nothing extra because the workflow already produces them, and eighteen months later they're the difference between "why does this code exist" and an archaeology project.
