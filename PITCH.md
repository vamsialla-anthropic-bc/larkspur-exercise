# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built: a Larkspur disruption-care chat agent on the Claude Messages API, with nine given tools plus next_available_day and fare_rules served over MCP.
Does: looks up the booking, reads the real flight status and the policy row, offers only rebookings the data supports, holds a seat for the customer to confirm, and hands abuse, legal threats, groups and refunds to a person.
Number: $0.0680 to $0.0259 model cost per contact (-62%), 5 ticket types, 3 runs each, n=15 per side, cold cache, same model; 15 of 15 still resolved
Safety check: 10 eval cases from the team, 5 of them hard gates; 5 of 5 hard gates pass and 8 of 10 cases pass overall on the cached agent, judged by a separate model.
Next: speed, which caching did not improve (p50 9.77 s to 10.63 s, inside run-to-run noise at n=15): shorter final answers and no repeat check_policy turn; then check every segment on a missed connection, and get the model to choose fare_rules for fare questions (today they are answered from check_policy instead).
Still broken: on a missed connection the agent checks only the Denver to Nashville flight and never the delayed Sacramento flight (conn-0101 fails).
Lever: cost

## Priya asked

Costs:
Wrong:
Runs it:
Left out:

## Most expensive turn

Turn 4 was the most expensive (4,767 tokens in, against 3,427 on turn 1) because every turn re-sends the whole conversation, so the last turn carries the system prompt, all nine tool schemas and every earlier tool result, including the large check_policy answer; the last turn would only cost less than the one before it if the unchanging start of the request (system prompt and tool schemas) were cached so it is billed at the cheaper cache-read rate, and if the tool results were trimmed to the fields Claude actually needs.

Build 4 did the first half. The clock line (runtime_preamble) sat at the very front of the system prompt and changed every second, so no request ever shared a prefix with the last one. It now sits after the stable prompt, the stable prompt and the newest message carry cache markers, and from turn 2 on the earlier context is read from cache: 89% cache hit, $0.0680 to $0.0259 per contact. Trimming the tool results is still undone.

