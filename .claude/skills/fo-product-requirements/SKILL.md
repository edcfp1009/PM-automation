---
name: "fo-product-requirements"
description: Turn a meeting, Fathom recording, Slack thread, brain dump, or existing Jira ticket into a short, plain-language Jira story for the FreightPOP FO board. Use this whenever Edward asks to write, open, draft, rewrite, clean up, or refine a ticket or story, or says things like "pull today's standup and open a ticket for X", "make this a ticket", "fix this ticket", even if he does not name the skill. Use this instead of product-requirements or fp-product-requirements.
---

# FO Product Requirements

Turn raw input into a short Jira story that engineers can read in two minutes.

## Why this skill exists

Earlier tickets were 10,000+ characters. They mixed decided and undecided items, filled every template section (often with N/A), and described how to build things. Engineers stopped reading them, and when they pasted a ticket into Claude, Claude started implementing the technical guesses in it. The fix, tested on FO-3922 and approved by the engineer, is:

- Only write what the team actually agreed on.
- Write what to build and why, in plain language. How to build it is the developer's job.
- Keep only the sections below. Leave everything else out.

**The test for every line:** could a non-engineer read this line and agree or disagree with it? If a line only makes sense to an engineer, cut it.

## Step 1: Gather input

Accept any of: a Jira key or URL, a Fathom recording (pull it with the Fathom tools), a meeting transcript, Slack text, or a brain dump.

If the user only says "pull today's standup" or similar, find the meeting with the Fathom tools and identify which action item the ticket is for.

## Step 2: List what was agreed, then stop

Before writing anything, show the user two short lists in the language they are chatting in:

- **Agreed:** decisions the people in the meeting clearly settled. One line each.
- **Not agreed:** anything discussed but left open, contradicted later in the meeting, or said once with hedging ("maybe", "let me think"). For each, quote or summarize both sides briefly so the user can decide fast.

Then wait. The user confirms, edits, or answers the open items.

Why: transcripts contain far more than the ticket needs. Ideas floated and dropped, side conversations, and early positions that were reversed later all end up in the ticket if the transcript goes straight into drafting. This step is the main reason the new tickets are short and correct.

**After confirmation, the confirmed list is the only source of requirements.** Do not add behavior, edge cases, or rules that are not on it. If you notice a real gap, ask the user as a question. Do not fill it yourself.

## Step 3: Light research

Do only what helps the user and the reader:

- Search Jira for the tickets this one depends on or builds on, so they can go in Related Context.
- Read the FreightPOP codebase through the GitHub MCP tools (repo `FreightPOP/FreightPOP`) only to make sure the `Today:` lines describe how the product works now. Do not copy technical findings into the ticket.
- If the code cannot be read, tell the user in chat and keep going. Write `Today:` only from what the meeting or Jira confirms. Never add inline tags like "(assumed, confirm with X)" to the ticket.

Do not search for extra context to pad the Backstory.

## Step 4: Draft

Write the ticket in English, plain language, no em dashes. Use exactly these sections, in this order. Do not add any other section.

```
Title: [Area] Short imperative description

## Backstory
2 to 5 sentences. What is wrong or missing today, what triggered this,
and any key decision the team made (and why, in one clause).
Mention another ticket only if the reader needs it to understand
the story. All other links go in Related Context.

## User Story
As a [persona], I want to [goal], so that [outcome].
One per persona. Two at most.

## Solution
For each area that changes:

**Navigate:** where in the product, starting from a named page
**Today:** how it works now (omit for a brand new screen)
**New:**
* plain-language bullets: what the user sees and what happens
* exact labels, toggle names, defaults
**Precedent:** an existing screen or ticket that does the same thing
(omit if none)

## Performance and Speed Requirements
Only for areas where speed matters to the user. One short line each,
as an outcome a non-engineer can check (for example, "the page loads
within 2 seconds"). If nothing in the ticket is speed-sensitive, say
so in one sentence.

## Dependencies and Risk Areas
What this ticket touches or waits on, and why it matters. One or two
plain sentences per item. Include other tickets it depends on, shared
parts of the product that could be affected, and anything that affects
every customer at once. No file names, class names, or fixes. If
nothing notable, say so in one sentence.

## Acceptance Criteria
**Short label**
GIVEN ...
WHEN ...
THEN ...

## Testing and Evaluation
Only for AI features.
Failure modes to cover: a short numbered list in plain language.
Eval: which Langfuse dataset to run.

## Scope Boundaries
* In scope: short list of what this ticket includes
* Out of scope: only items the team explicitly said are not in this
  ticket (omit the line if none)

## Additional Considerations
Things the developer should keep in mind while building. Not blockers,
not questions, nothing anyone has to answer before starting.
Omit the section if there is nothing worth flagging.

## Estimate
Only if an engineer gave one. Note who and when.

## Related Context
* TICKET-123: one line on how it relates
* Recording or page links
```

### Section rules

**Acceptance Criteria**
- One behavior per criterion. Usually 4 to 8 in total.
- Each one must trace back to an item on the confirmed list.
- Do not add permission, error, or edge-case criteria by default. Add them only if the team discussed that behavior.
- Do not write the same behavior twice with different wording.

**Testing and Evaluation**
- Do not write manual test steps. Developers and QA handle that themselves.

**Performance and Dependencies**
- Austin's automated PM Review fails tickets without these two sections, so always include them, even if only as one sentence.
- These two sections can come from your own research (code, Jira), not only from the confirmed list. When showing the draft, mark which items came from research so the user can confirm them.
- Never name files, classes, or methods. Describe the product area instead (for example, "the usage service that records AI cost").

**Additional Considerations**
- Risks and things that could break go in Dependencies and Risk Areas. This section is only for small notes the developer should keep in mind while building.
- Never phrase these as requirements or as conditions for starting work.

### Never include

Implementation steps, architecture, technical notes, logging, analytics events, permissions sections, third-party or provider details, configuration or storage details, authentication, feature flag plans, N/A lines, or "to confirm at grooming" notes. If something truly needs an answer before work starts, ask the user in chat instead.

## Step 5: Check before showing

Run these checks and fix what you find:

1. **Contradictions:** compare criteria in pairs. Same situation with different outcomes, or a criterion that an earlier one makes impossible, gets fixed if the confirmed list makes the answer clear. Otherwise ask the user.
2. **Duplicates:** merge criteria that describe the same behavior.
3. **Consistency:** Solution, Performance, Dependencies, Acceptance Criteria, and Scope Boundaries must say the same thing. For example, if Solution adds a toggle, Scope Boundaries cannot say "no UI".
4. **Plain language:** remove any line only an engineer would understand.
5. **Source:** remove anything not on the confirmed list.

## Step 6: Show, then write to Jira

Show the full draft in chat. Point out anything you were unsure about in one or two sentences, and list which Performance and Dependencies items came from your own research rather than the confirmed list. Create or update the Jira ticket only after the user says to. When updating an existing ticket, replace the description and title, and leave other fields alone.

### Jira fields to set when creating a ticket

A new ticket must have these fields filled, not just the title and description. Show the values you plan to use together with the draft, so the user can confirm them in the same step.

| Field | Jira field | What to set |
| --- | --- | --- |
| Issue type | issuetype | Story, unless the user says otherwise |
| Assignee | assignee | The engineer who will build it |
| Product Owner | customfield_10277 | Edward (EdwardC) |
| Primary Product Segment | customfield_10294 | Copy from the closest related ticket. If there is none, ask. |
| Development Type | customfield_10302 | Copy from the closest related ticket. If there is none, ask. |
| Sprint | customfield_10103 (sprint id) | The sprint the team agreed on. If not discussed, ask. |
| Labels | labels | `Sprint_<number>` matching the Sprint field, plus topic labels copied from the closest related ticket |
| Original estimate | timetracking.originalEstimate | Only if an engineer gave one. Same number as the Estimate section. |

Rules:

- The `Sprint_<number>` label and the Sprint field must always point to the same sprint.
- To find the sprint id, read the Sprint field of a ticket already in that sprint. If the agreed sprint ends today or has already ended, point that out and ask whether to use the next sprint.
- When updating an existing ticket, only change these fields if the user asked to (for example, moving it to another sprint or changing the estimate).
- After writing, tell the user which values you set and where each one came from.

## Example

A shortened version of FO-3922 after the rewrite:

```
Title: [AI Platform] Add a Manual Switch to Move AI Features from Bedrock to OpenAI GPT

## Backstory
AI Copilot is moving from OpenAI to Amazon Bedrock (FO-3889), and Load
Planning, Audit Agent, and AI Invoice Import will follow (FO-3917).
Today, if Bedrock goes down, every AI feature fails for every customer.
At the 10/6 grooming, the team decided against automatic fallback
because detecting an outage and switching back safely is hard to get
right. Instead, FreightPOP gets one manual switch.

## User Story
As a FreightPOP internal admin, I want to move all AI features from
Bedrock to OpenAI GPT with one switch, so that customers can keep using
AI features while Bedrock is down.

## Solution
**Navigate:** FreightPOP internal AI admin, the page where the active
model is changed (FO-3894)
**Today:** Copilot always calls Bedrock. If Bedrock is down, it fails.
**New:**
* Add a toggle labeled "Stop using Bedrock". Default: off.
* When on, all four features use the GPT model each one used before
  the Bedrock move. When off, they go back to Bedrock.
* One toggle for all four features. Only internal admins can see it.

## Performance and Speed Requirements
After an internal admin changes the toggle, the next AI call from
every feature uses the new provider, with no deploy or restart.

## Dependencies and Risk Areas
* Waits on FO-3917. The other three features must be on Bedrock
  before the switch can cover them.
* One toggle changes AI behavior for every customer at once. Turning
  it on by mistake moves all traffic to GPT and changes cost for
  every company.
* Prompts are being tuned for Bedrock, so a feature may answer
  differently or fail on GPT.

## Acceptance Criteria
**Switch to GPT**
GIVEN an internal admin turns the toggle on
WHEN any of the four features makes an AI call
THEN the call goes to that feature's GPT model and the user sees no error

## Scope Boundaries
* In scope: one manual toggle for Copilot, Load Planning, Audit Agent,
  and AI Invoice Import; usage recorded under the model that answered
* Out of scope: automatic fallback; per-feature switches

```
