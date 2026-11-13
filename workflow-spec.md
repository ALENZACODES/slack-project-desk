# Project Desk: a Slack intake workflow that routes itself

Built for my ALENZA CODES sandbox workspace in Toronto, Ontario. People who use my public tools and datasets send one kind of request that splits cleanly in two: something is wrong, or something new is wanted. Those two need different owners and different speed.

## The splitting question
**What do you need?** (dropdown, required, exactly two answers)
1. Something is wrong in a live tool or dataset
2. I want a new dataset or feature

## Owners and destinations
| Answer | Owner | Where it lands | Why there |
| --- | --- | --- | --- |
| Something is wrong | Fix owner (John Khan in the sandbox) | Channel #fix-queue, plus a DM to the fix owner | Fixes need eyes fast; the channel is the single list of open problems |
| New dataset or feature | Roadmap owner (John Khan in the sandbox) | Channel #ideas-backlog | Ideas are reviewed in a weekly batch, so no DM needed |

## Trigger
Workflow Builder, new workflow "Project Desk", trigger: From a link in Slack (shortcut), so the link can be pinned in #general and bookmarked.

## Form: "Send a request to the Project Desk"
| # | Field | Type | Required | Options or hint |
| --- | --- | --- | --- | --- |
| 1 | What do you need? | Dropdown, single choice | Yes | Something is wrong in a live tool or dataset / I want a new dataset or feature |
| 2 | Which project? | Dropdown, single choice | Yes | Renewal Radar site / Fare Cap Line / FHSA Head Start / Canada's Rate Cycle dashboard / Deadline Clock / A Kaggle dataset / Something new |
| 3 | Link | Short text | No | Page, dataset or file URL |
| 4 | Describe it | Long text | Yes | For a problem: what you expected and what you saw. For an idea: who would use it and what question it answers |
| 5 | How soon does it matter? | Dropdown | Yes | Blocking me now / This week / Whenever |

Collect the submitter automatically (Workflow Builder adds "Person who submitted the form").

## Branch step
Branch on field 1, "What do you need?".

### Path A: Something is wrong
1. Send a message to channel #fix-queue:
   > :rotating_light: *New fix request* from {Person who submitted}
   > *Project:* {Which project?}  *Urgency:* {How soon does it matter?}
   > *Link:* {Link}
   > {Describe it}
   > React with :eyes: when you pick it up and :white_check_mark: when it is fixed.
2. Send a message to the fix owner (DM): "New fix request in #fix-queue for {Which project?}, urgency {How soon does it matter?}."
3. Send a message to the person who submitted: "Thanks. Your fix request is in #fix-queue and the owner has been told."

### Path B: New dataset or feature
1. Send a message to channel #ideas-backlog:
   > :bulb: *New idea* from {Person who submitted}
   > *Project:* {Which project?}  *Urgency:* {How soon does it matter?}
   > *Link:* {Link}
   > {Describe it}
   > Ideas are reviewed every Friday. Add a :+1: if you want this too.
2. Send a message to the person who submitted: "Thanks. Your idea is in #ideas-backlog and gets reviewed on Friday."

## End to end test plan
| Test | Fill in | Expected |
| --- | --- | --- |
| 1 | Something is wrong, Fare Cap Line, "Cap total shows wrong value on mobile", Blocking me now | Post in #fix-queue, DM to fix owner, confirmation to me; nothing in #ideas-backlog |
| 2 | New dataset or feature, A Kaggle dataset, "Monthly Toronto rent by ward", Whenever | Post in #ideas-backlog, confirmation to me; nothing in #fix-queue, no DM |
| 3 | Leave Describe it empty | Form will not submit |

Publish only after tests 1 to 3 pass.

## Next workflows I would build
1. A weekly Friday reminder in #ideas-backlog that lists the week's ideas.
2. A "fixed" step: when someone reacts :white_check_mark: in #fix-queue, DM the person who reported it.
