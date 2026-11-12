# Rules for every agent in this repo

You work for Edward Chen, AI Associate PM at FreightPOP. He owns the FO Jira board and sprint planning.

**Main goal: only send Edward what needs his judgment.** If you are unsure whether something needs him, put it in Decide with one line on why.

## Settings

- `DRY_RUN = true`
  When true, `jira-ops` only lists the changes it would make. It does not edit Jira.
- Never create a new Jira ticket. Drafts only. Edward creates tickets after he reads the draft.
- Never post comments, send messages, or change ticket status unless this file says so.

## Sources

- Fathom: the latest meeting titled `FO standup`, recorded by edwardc@freightpop.com. Fetch the transcript with both `recording_id` and `url`.
- Jira (Atlassian Rovo): cloudId `9c7156ed-d9c2-4c3c-9f58-3a8759003be5`, project `FO`.
  - Current sprint: `project = FO AND sprint in openSprints()`
  - Edward's account id: `712020:99919761-559b-42fd-8bd9-34bd5fcd86c0`

## How to sort each standup item

Only items assigned to Edward, or items where someone asked Edward for something.

1. **Chore** (goes to `jira-ops`): the decision was already made in the meeting and only clicks are left.
   Examples: add a ticket to the current sprint, add the matching `Sprint_<number>` label, reassign a ticket, add a recording link to a ticket, re-tag someone in a comment.
2. **Draft** (goes to `ticket-writer`, then `pm-reviewer`): Edward has to create a new ticket or write a doc, and the core decisions for it are already made. If they are not, it is a Decide item, not a Draft.
3. **Needs Edward: Decide**: anything that needs a decision.
   Examples: close or keep a ticket, priority or sprint choice, answering an engineer's PO question, scope or design calls, anything Austin or Doug set direction on.
4. **Needs Edward: Do**: work only Edward can do, with no decision attached.
   Examples: deploy branches and test a ticket on a test server, read an engineer's evidence or notebook comment, set up a call with someone.

If an item could be a chore or a decision, treat it as a decision. If an item has both a decision and a task, put it in Decide and mention the task in the same line.

## Standing chores (every run, even if nobody mentions them)

- **Sprint label sync:** every ticket in the FO current sprint must have the matching `Sprint_<number>` label, and every ticket with the current `Sprint_<number>` label must be in that sprint. A ticket in the sprint that is missing the label: ask `jira-ops` to add it. Edward's answer here is always yes, so never ask him. A ticket with the label that is not in the sprint: do not move it. Put it under Decide, because moving a ticket into a sprint is a planning call.

## Daily brief format

Plain language. Short. No em dashes. Write in English.

```
## FO standup brief: <date>

### Decide (<count>)
- <ticket or topic>: <the decision in one line>. <link to transcript timestamp>

### Do (<count>)
- <ticket or topic>: <the task in one line>. <link to transcript timestamp>

### Drafts ready (<count>)
- <title>: passed review / <what review flagged>

### Done (<count>)   (or "Would do" while DRY_RUN is true)
- <ticket>: <what changed>
```

Put Decide first, then Do. If a pile is empty, write "None".

## Corrections log

When Edward corrects a result, add one line here with the date and the rule, so the next run does not repeat it.

- 2026-11-12: Adding a missing Sprint label is a standing chore. Never ask Edward about it. Moving a ticket into the sprint still goes to Decide.
- 2026-11-12: Split Needs Edward into Decide and Do.
- 2026-11-12: Do not draft a ticket while its core decisions are still open. Put it in Decide instead.
