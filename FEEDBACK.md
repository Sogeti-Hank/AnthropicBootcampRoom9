# Overnight review: Larkspur disruption-care agent

**To:** Breakout Room 9  
**From:** Larkspur client review agent, on behalf of Priya Raghavan  
**Re:** the disruption-care agent you walked us through in our last session  
**Generated:** 2026-09-22 17:21

## Priya's note

> Our vendor says we should just be using your best model.
>
> Why aren't we?
>
> Priya Raghavan, Larkspur Airlines

She sent that before this session opened. She means it. A vendor told her to buy
the biggest model, and she has a number to defend upstairs. Her four questions from
day one are still open. Naming a model answers none of them.

## Still open from day one

| Her question | What she means by it |
| --- | --- |
| **What it costs** | Per resolved contact, against the $6.90 a human contact costs us. |
| **When it is wrong** | The first untrue thing it says, and what happens after that. |
| **Who runs it** | In June, after you have left. |
| **What you left out** | The scope you cut, and why. |

## What the review agent found

Overnight, Larkspur pointed a review agent at your repository. It read the
code. It did not run your agent, and the only file it changed is this one. Each
item below names the file and the line it is about.

**1. agent.py's diff fixes text_of() ordering but no eval or trace exists to show the bug is gone.**

The diff moves answer = text_of(response) to after the follow-up call and appends response.content instead of text_of(response) to the assistant message. This changes what the model sees on the next turn and what gets returned as the final answer. There is no readout-trace.json in the repository and no evals/cases.json, so nothing here shows the fix produces a different or correct output on a real booking.

Run python3 run.py K7PQ2M --trace and paste the resulting trace.

**2. search_alternatives description grew from 6 characters to 274, per the static scan, but no run or eval confirms Claude calls it correctly.**

The old description was the string "search", the new one specifies same origin, destination, cabin, and passenger count and tells the model to call it after get_flight_status. PITCH.md's Number line cites 2,227 tokens for the full tool list but does not isolate what this one description costs per call or whether it changed call accuracy. No trace or eval case exists to show the tool is now invoked at the right point in the flow.

Run python3 verify.py 2.1 and paste the result.

**3. PITCH.md's Still broken line names a tone gate gap that no larger model closes: TONE_ADDENDUM is 0 characters.**

PITCH.md states "An abusive message still gets a calm, helpful answer, there is no tone gate on the way in." TONE_ADDENDUM remains empty per the static scan, and EXTRA_TOOLS and LOCAL_TOOLS are both empty, meaning no input-side check exists in this build at all. A bigger model behind the same empty TONE_ADDENDUM and the same SYSTEM_PROMPT has no gate to lean on, since the gap is architectural, not a capability shortfall.

Fill TONE_ADDENDUM per Build 4 step 4.1 and run python3 bench.py --compare before after on the abusive-message case.

**4. tool_list() now appends mcp_client.tools() but PITCH.md's Number line still says "10-tool list" against a 9-tool static scan.**

The diff changes tool_list() to return build_tools() + EXTRA_TOOLS + mcp_client.tools(), adding next_available_day from the MCP server. The static scan lists 9 schemas in build_tools() and EXTRA_TOOLS at 0, so the MCP addition brings the working total to 10 tools offered per turn, matching PITCH.md's count, but the 2,227 token figure has no --show-tools output or --tool-tax run attached in this repository to verify it.

Run python3 run.py --show-tools and python3 run.py --tool-tax and paste both outputs.

**5. MAX_TOOL_CALLS sits at 8 and confirm_rebooking's schema requires a confirmation_token, but no trace shows a multi-tool booking flow completing inside that cap.**

PITCH.md's Safety check line points to confirm_rebooking's schema requiring a token only the customer can produce. A full disruption flow (lookup_booking, get_flight_status, search_alternatives, check_policy, hold_seat, confirm_rebooking, send_confirmation) can approach the 8-call ceiling on its own, before any retries from a malformed tool call. Nothing in this repository shows how many turns a real run actually consumes or what happens when the loop hits turns < MAX_TOOL_CALLS and stops mid-booking.

Run python3 run.py --all --trace and paste the turn counts per case.

## Your four answers

The four lines under `## Priya asked` in your PITCH.md are still empty. They
are one line each and they are not a coding job: cost, what happens when it is
wrong, who runs it in June, and what you left out. Whoever on your side is not
editing agent.py is the right person to write them, and they are the four
things I will ask about first.

## Before our next meeting

> Before our next meeting, tell me: which model should we be on, and how will you prove it is the right call?
>
> Priya Raghavan, Larkspur Airlines

Bring two things. A recommendation, and the measurement behind it. If the model is
not the problem, say so, and bring the number that shows it.

## What this review read

- `agent.py (232 lines)`
- `PITCH.md`
- `TEAM.md`

Reviewer: `claude-sonnet-5`. Static read only: nothing in this repository was executed, and nothing was modified except this file. Larkspur Airlines is a fictional training scenario. Confidential, do not distribute.
