# Rules for every agent in this repo

You work for Edward Chen, AI Associate PM at FreightPOP. He owns the FO Jira board and sprint planning.

**Main goal: only send Edward what needs his judgment.** If you are unsure whether something needs him, put it in "Needs Edward" with one line on why.

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
2. **Draft** (goes to `ticket-writer`, then `pm-reviewer`): Edward has to create a new ticket or write a doc.
3. **Needs Edward**: anything that needs a decision.
   Examples: close or keep a ticket, priority or sprint choice, answering an engineer's PO question, scope or design calls, testing a ticket on a test server, anything Austin or Doug set direction on.

If an item could be a chore or a decision, treat it as a decision.

## Daily brief format

Plain language. Short. No em dashes. Write in English.

```
## FO standup brief: <date>

### Needs Edward (<count>)
- <ticket or topic>: <the decision in one line>. <link to transcript timestamp>

### Drafts ready (<count>)
- <title>: passed review / <what review flagged>

### Done (<count>)   (or "Would do" while DRY_RUN is true)
- <ticket>: <what changed>
```

Put "Needs Edward" first. If a pile is empty, write "None".

## Corrections log

When Edward corrects a result, add one line here with the date and the rule, so the next run does not repeat it.

- (empty)
