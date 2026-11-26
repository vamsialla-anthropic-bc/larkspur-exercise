# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: a Larkspur disruption-care chat agent on the Claude Messages API, with nine given tools plus next_available_day and fare_rules served over MCP.
Does: looks up the booking, reads the real flight status and the policy row, offers only rebookings the data supports, holds a seat for the customer to confirm, and hands abuse, legal threats, groups and refunds to a person.
Number: $0.0685 model cost per contact (p50 10.05 s, 17,805 tokens in per contact), 5 ticket types, 3 runs each, n=15
Safety check: 10 eval cases from the team, 5 of them hard gates; 5 of 5 hard gates pass and 7 of 10 cases pass overall, judged by a separate model.
Next: check every segment on a missed connection, get the model to choose fare_rules for fare questions (today they are answered from check_policy instead), and cut the repeat check_policy call that adds a turn.
Still broken: on a missed connection the agent checks only the Denver to Nashville flight and never the delayed Sacramento flight (conn-0101 fails).
Lever: intelligence

## Priya asked

Costs:
Wrong:
Runs it:
Left out:

## Most expensive turn

Turn 4 was the most expensive (4,767 tokens in, against 3,427 on turn 1) because every turn re-sends the whole conversation, so the last turn carries the system prompt, all nine tool schemas and every earlier tool result, including the large check_policy answer; the last turn would only cost less than the one before it if the unchanging start of the request (system prompt and tool schemas) were cached so it is billed at the cheaper cache-read rate, and if the tool results were trimmed to the fields Claude actually needs.
