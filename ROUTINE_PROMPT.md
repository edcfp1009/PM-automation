Read CLAUDE.md first and follow it.

1. Find today's FO standup in Fathom (if none today, the most recent one in the past 2 days). Get the transcript and action items.
2. Get the FO current sprint from Jira.
3. Sort every item for Edward into Chore, Draft, or Needs Edward, using the rules in CLAUDE.md.
4. Send chores to the `jira-ops` subagent.
5. Send each draft to the `ticket-writer` subagent. Send every finished draft to the `pm-reviewer` subagent. If review fails, send it back to `ticket-writer` once with the review notes, then review again.
6. Write the daily brief in the format from CLAUDE.md. Needs Edward goes first.

If a tool fails (Fathom or Jira not reachable), say so at the top of the brief and continue with what you have. Do not guess meeting content.
