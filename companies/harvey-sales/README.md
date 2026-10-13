# Harvey Sales

An outbound sales team for [Paperclip](https://github.com/paperclipai/paperclip), modeled on [Harvey](https://github.com/ethanplusai/harvey), the autonomous sales agent that finds businesses worth selling to, writes cold email that references something real about each one, and works replies toward a meeting.

Harvey does this as a single Python process driving the Claude CLI. This package expresses the same loop as five Paperclip agents, a project with starter and recurring tasks, and nine pinned references to Harvey's sales playbooks. The approval gate that Harvey implements as an outbox becomes a Paperclip approval request, so nothing is sent without a human saying yes.

## How the team works

The pipeline follows Harvey's deterministic heartbeat rule:

```
handle replies  >  send approved mail  >  write campaigns  >  prospect  >  analyze
```

1. **Sales Lead** runs the heartbeat every 15 minutes during working hours, reads four pipeline counts, and dispatches exactly the work they call for.
2. **Scout** discovers businesses for a cohort (a query over confirmed signals), profiles them from their own websites, finds the decision-maker, verifies the address, and scores each prospect against the ICP.
3. **Writer** turns qualified prospects into three-email sequences under strict length, phrasing, and one-question rules.
4. **Approval gate.** The Sales Lead raises an approval request for every campaign and every reply batch. The board approves or rejects.
5. **Handler** sweeps replies twice an hour, classifies intent, advances the conversation stage, honors opt-outs instantly, escalates hostile or legal replies to a human, and drafts responses that go back through the gate.
6. **Analyst** reviews the pipeline every Monday and recommends changes to the cohort, the angle, or the limits.

## Org chart

| Agent | Title | Reports to | Skills |
|---|---|---|---|
| `sales-lead` | Head of Outbound Sales | none | sales-methodology, signal-playbook |
| `scout` | Prospecting and Qualification | sales-lead | prospecting-tactics, lead-qualification, account-navigation, signal-playbook |
| `writer` | Outbound Copywriter | sales-lead | email-frameworks, signal-playbook, sales-methodology, offer-strategy |
| `handler` | Reply Handling and Conversations | sales-lead | objection-handling, sales-methodology, offer-strategy |
| `analyst` | Pipeline Analyst | sales-lead | none |

## Project and tasks

`outbound-pipeline` is the single project. It ships with:

| Task | Assignee | Kind | Schedule |
|---|---|---|---|
| Train the team on the product | sales-lead | starter | once |
| Confirm signals and ICP | scout | starter | once |
| Pipeline heartbeat | sales-lead | recurring | every 15 minutes, 07:00 to 22:59 New York |
| Reply sweep | handler | recurring | twice an hour, 07:00 to 22:59 New York |
| Weekly pipeline review | analyst | recurring | Mondays 09:00 New York |

Schedules live in `.paperclip.yaml` and can be changed after import.

## Skills

All nine skills are **referenced**, not copied. Each `skills/<slug>/SKILL.md` carries a `metadata.sources` entry pinned to commit `822e84e7` of `ethanplusai/harvey` with the file's sha256, attribution to Ethan Rogers, and the upstream MIT license. The Harvey repo's `skills/README.md` explains how each file is meant to be edited.

| Skill | Upstream file |
|---|---|
| prospecting-tactics | `skills/prospecting_tactics.md` |
| lead-qualification | `skills/lead_qualification.md` |
| account-navigation | `skills/account_navigation.md` |
| signal-playbook | `skills/signal_playbook.md` |
| email-frameworks | `skills/email_frameworks.md` |
| sales-methodology | `skills/sales_methodology.md` |
| offer-strategy | `skills/offer_strategy.md` |
| objection-handling | `skills/objection_handling.md` |
| linkedin-outreach | `skills/linkedin_outreach.md` |

Harvey also generates two product-specific skills (`product_knowledge.md` and `competitive_intel.md`) with `harvey train`. They are gitignored upstream, so this package replaces them with the "Train the team on the product" starter task, which produces the equivalent brief as a project document.

## Getting started

Preview the import, then apply it as a new company:

```sh
paperclipai company import companies/harvey-sales --dry-run
paperclipai company import companies/harvey-sales --target new --yes
```

Then, in order:

1. Let the Sales Lead run "Train the team on the product" and answer its questions.
2. Let the Scout run "Confirm signals and ICP" and approve the signals you want collected.
3. Enable the recurring routines. The first heartbeat will dispatch discovery against the cohort.
4. Approve or reject each campaign and reply batch as it arrives.

No adapter is pinned in `.paperclip.yaml`, so pick the runtime for each agent at import time.

## Hard rules

Opt-outs are permanent. Nothing is fabricated. Nobody is impersonated. Hostile, legal, and compliance replies go to a human. Nothing is sent without approval. These are stated in `COMPANY.md` and repeated in the agents that need them.

## References

- Source: [ethanplusai/harvey](https://github.com/ethanplusai/harvey) (MIT, Ethan Rogers)
- Format: [Agent Companies specification](https://agentcompanies.io/specification)
- Runtime: [Paperclip](https://github.com/paperclipai/paperclip)

Generated with the company-creator skill from Paperclip.
