# PITCH.md

Six lines and a lever. Your words. The last two are scored.

Built:
Does:
Number:
Safety check:
Next:
Still broken:
Lever: <cost | speed | intelligence>

## Priya asked

Costs:
Wrong:
Runs it:
Left out:

## Most expensive turn

Turn 4 was the most expensive (4,767 tokens in, against 3,427 on turn 1) because every turn re-sends the whole conversation, so the last turn carries the system prompt, all nine tool schemas and every earlier tool result, including the large check_policy answer; the last turn would only cost less than the one before it if the unchanging start of the request (system prompt and tool schemas) were cached so it is billed at the cheaper cache-read rate, and if the tool results were trimmed to the fields Claude actually needs.
