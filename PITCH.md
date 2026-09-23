# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: We built the loop, tool routing, and the MCP handoff for next_available_day.
Does: It looks up the disrupted flight and provides future flight options.
Number: 2,227 tokens per turn, for the full 10-tool list.
Safety check: confirm_rebooking requires a confirmation_token only the customer can produce by taking a prescribed action, enforced in the tool's own schema/description.
Next: Add a tone gate for abusive messages.
Still broken: An abusive message still gets a calm, helpful answer — there is no tone gate on the way in.
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:
