# DRAFT (not created in Jira): [AI Invoice Import] Count AI Invoice Import Use Against the Company AI Budget and Show It in Usage

## Backstory
All AI agents in the AI Control Center share one company AI budget, and their use shows on the AI Control Center Usage page. AI Invoice Import was recently moved to our new AI provider setup (FO-3945), but its usage was not moved with it, so it still runs on its own token system. At the 11/12 FO standup, Aitezaz said it is the only AI feature left that does not use the shared budget.

## User Story
As a company admin, I want AI Invoice Import use to come out of the same AI budget as my other agents and show on the Usage page, so that I can see all of my AI spend in one place.

## Solution
**Navigate:** AI Control Center > Usage
**Today:** AI Invoice Import uses its own token system. Its use is not counted in the company AI budget and does not appear on the Usage page.
**New:**
* Every AI Invoice Import AI call uses the same company AI budget the other agents use.
* AI Invoice Import no longer uses its own token system.
* AI Invoice Import use shows on the Usage page as its own agent, the same way the other agents do.

**Precedent:** Audit Agent and Load Planning on the shared AI budget (FO-3777); Tracking Agent on the shared budget (FO-4011)

## Performance and Speed Requirements
* AI Invoice Import use shows on the Usage page as quickly as the other agents' use does today.
* Importing an invoice takes about as long as it does today.

## Dependencies and Risk Areas
* Builds on FO-3945 (AI Invoice Import on the new AI provider setup), which is In Testing. This ticket changes the same feature, so FO-3945 should be finished first.
* The shared budget lives in the usage service, so that service needs changes too, not only AI Invoice Import.
* The shared budget is used by every company with AI turned on. A mistake here could charge the wrong agent or the wrong company.
* Under the shared budget, a company with no AI budget set cannot use AI (FO-3777). Companies that use AI Invoice Import today but have no budget could lose invoice import when this ships. What AI Invoice Import does when a budget runs out or was never set is decided separately (see Scope Boundaries), and that decision must be made before this ships.
* Under the shared budget, AI is blocked whenever the budget service cannot be reached (FO-3777). Once AI Invoice Import uses it, an outage of that service stops invoice import for every customer at once.

## Acceptance Criteria
**Uses the shared budget**
GIVEN a company with AI Invoice Import turned on
WHEN AI Invoice Import reads an invoice
THEN the cost comes out of that company's AI budget

**Old token system no longer used**
GIVEN a company with AI Invoice Import turned on
WHEN AI Invoice Import reads an invoice
THEN its own token system is not used and no Invoice Import tokens are taken off

**Shows in Usage**
GIVEN AI Invoice Import has read invoices for a company
WHEN an admin opens AI Control Center > Usage
THEN AI Invoice Import use is listed as its own agent

## Scope Boundaries
* In scope: AI Invoice Import use counted in the shared company budget; AI Invoice Import no longer using its own token system; AI Invoice Import shown on the Usage page
* Out of scope: Tracking Agent on the shared budget (FO-4011); moving AI Invoice Import to the new AI provider setup (FO-3945)
* Out of scope, decided separately: whether companies' existing AI Invoice Import token balances carry over to the shared budget; what AI Invoice Import does when a company's budget runs out or was never set

## Related Context
* FO-3945: AI Invoice Import on the new AI provider setup. Usage tracking was left for a separate ticket.
* FO-4011: Tracking Agent on the shared budget. Same change for another agent.
* FO-3777: AI credit service and shared company budget
* FO-3609: AI Control Center usage view
* FO-3737: AI Control Center epic
* FO-4000: Contract credits, margin, and top-ups on the AI credit budget
* FO-3775: Earlier wiring ticket that listed "Invoice PDF"
* Recording: [FO standup 11/12](https://fathom.video/calls/897877545?timestamp=501)

---
## Open questions for Edward (not in the ticket body)
- Scope note added at the reviewer's request: the "Out of scope, decided separately" line was not decided in the meeting. Nobody said these two items are out of this ticket, and the skill only allows out-of-scope items the team named. Before creating the ticket, confirm that line, or answer the two questions below and move the answers into the ticket.
- Existing Invoice Import token balances: carry over, or enter budgets by hand like FO-3777 did? Where is this decided?
- Budget runs out / no budget set: block Invoice Import like the other agents? If yes, budgets must be set before release. The Dependencies line now says this must be decided before the ticket ships.
- Sprint: Aitezaz said "next sprint, or maybe I have some time in this sprint". Not settled. Sprint 145 ends 11/20; Edward is out next sprint.
- FO-3775 (Done) lists "Invoice PDF" wired to the credit service. Same feature? If yes, link them.
- Assignee: likely Aitezaz (owns FO-3945). Not assigned in the meeting.
- Fields: Product Segment "Freight Invoice Audit" (like FO-3945) or "AI Agent" (like FO-4011); Dev Type "Product Roadmap" or "Tech Debt".
- Writer's own additions (not from the meeting): "import takes about as long as today", FO-3945 ordering, "no budget means no AI" risk, "budget service outage blocks invoice import for everyone" risk (both from FO-3777).
