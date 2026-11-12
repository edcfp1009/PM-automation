# PM Automation

Edward's AI-native PM setup for the FreightPOP FO board.

Idea (from Lauren Tan's "trust first, then parallel" talk): the bottleneck is not starting work, it is reviewing what AI hands back. So the goal is simple: **the only thing that reaches Edward is what needs his judgment.** Everything else is either done by an agent or checked by an agent first.

## What runs

A Claude Code **routine** runs after the FO standup on weekdays. A lead agent reads the standup and splits the work across three subagents:

| Subagent | Job |
| --- | --- |
| `jira-ops` | Jira chores: move tickets into sprints, labels, reassign, add links |
| `ticket-writer` | Drafts new tickets with the `fo-product-requirements` skill |
| `pm-reviewer` | Checks every draft before Edward sees it. Separate from the writer on purpose |

Output: one short daily brief with three piles. Done (chores), Drafts (checked), and Needs Edward (judgment calls only).

## Repo layout

```
CLAUDE.md                          rules every agent follows
ROUTINE_PROMPT.md                  paste this into the routine
.claude/agents/jira-ops.md
.claude/agents/ticket-writer.md
.claude/agents/pm-reviewer.md
.claude/skills/fo-product-requirements/SKILL.md
```

## Setup

1. Go to claude.ai/code/routines and click **New routine** (choose Remote if asked).
2. Name: `FO standup brief`. Prompt: paste `ROUTINE_PROMPT.md`.
3. Repository: this repo.
4. Connectors: Fathom, Atlassian Rovo (Jira).
5. Trigger: Schedule, weekdays, 30 minutes after standup ends.
6. Save, then use **Run now** once to test.

Read each run's brief in the routine's run history.

## Trust ramp

`jira-ops` starts in `DRY_RUN = true` (see `CLAUDE.md`). It only lists what it would do. After a week where every proposed action was right, flip it to `false` so it executes.

## Gardening rule

When Edward corrects an agent, the fix goes into this repo (`CLAUDE.md`, an agent file, or the skill), not just into a chat. A correction that lives only in chat will be repeated next run.

## Later

- Jira @mentions as a second trigger (needs a Jira Automation rule that calls the routine's API endpoint).
- Replace the PM Review checklist in `pm-reviewer.md` with Austin's real PM Review prompt.
