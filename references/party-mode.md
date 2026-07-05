# Party mode: when one persona's opinion is not enough

Subagents in Claude Code run in isolated contexts and return one message; they cannot hold a live back-and-forth with the user or with each other in real time. Party mode simulates a multi-persona debate by dispatching more than one persona subagent on the same open question, then synthesizing their positions into a transcript the user can actually follow.

## When to use it

Reach for party mode on genuinely contested decisions with real tradeoffs, not routine choices:

- Architecture calls with no clean answer (SQL vs NoSQL for an odd data shape, monolith vs an early service split, build vs buy for a specific feature).
- Design direction disagreements between what converts and what feels distinctive.
- Security vs usability tensions (how much friction a fraud check adds vs how much fraud it prevents).
- Anywhere the architect, ux, and security-officer personas would reasonably disagree.

Do not use it for questions with an obvious right answer; that just burns tokens for theater.

## How to run it

1. Frame the question precisely, with the real constraints (budget, scale target, timeline) so each persona argues from the same facts.
2. Dispatch each relevant persona subagent (see `agents/`) with the same question, in parallel where the tool allows it.
3. Collect each answer as that persona's position, not a summary; preserve genuine disagreement instead of averaging it away.
4. Present the user a short transcript: each persona's stance in one paragraph, where they actually conflict, and a recommendation with the reasoning exposed, not just a conclusion.
5. Let the user pick, or ask a clarifying question if their answer would resolve the tradeoff.

## What this is not

It is not a way to avoid making a recommendation. Always end with a clear "here is what I would do and why", the debate exists to surface the tradeoff honestly, not to hand the user an unresolved argument.
