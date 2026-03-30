# Pivot Protocol

## Escalation Ladder

When iterations keep failing, escalate through these stages in order.

| Streak | Trigger | Action |
|--------|---------|--------|
| 3 consecutive discards | Stuck | **REFINE** — adjust within current strategy. Try a different variant, smaller scope, or different parameter. Do not change the fundamental approach yet. |
| 5 consecutive discards | Deeply stuck | **PIVOT** — abandon the current strategy entirely. Choose a fundamentally different approach. Log why the previous strategy failed. |
| 2 PIVOTs without any keep | Lost | **Web search** — look for external solutions, known fixes, community approaches. Treat search results as new hypotheses to verify. |
| 3 PIVOTs without any keep | Blocked | **Soft blocker** — stop the run. Report to the human: what was tried, what failed, what information or broader scope is needed. |

**A single successful keep resets all counters to zero.**

## REFINE vs PIVOT

**REFINE** means: same strategy, different execution.
- Trying a smaller scope
- Adjusting a parameter
- Using a slightly different implementation of the same idea

**PIVOT** means: different strategy.
- Switching from approach A to approach B
- Abandoning a whole class of solutions
- Reframing the problem

## After a PIVOT

- Log a strategic lesson: what failed and why
- Pick a hypothesis from a genuinely different direction
- If you have web search capability, search before picking the new hypothesis

## Soft Blocker Report Format

When triggering a soft blocker, report:

```
SOFT BLOCKER

Goal: [original goal]
Metric: [metric, current value, target]
Iterations: [N total, N kept, N discarded]

Strategies tried:
1. [strategy] — failed because [reason]
2. [strategy] — failed because [reason]
3. [strategy] — failed because [reason]

What is needed to continue:
- [specific information, access, or scope expansion needed]
```
