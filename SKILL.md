---
name: autoresearch-loop
description: Runs an autonomous modify-verify-decide loop toward a measurable goal. Use when an agent needs to iterate repeatedly on a codebase, research task, or any problem with a mechanical metric — test coverage, type errors, lint warnings, performance, research quality. Keeps improvements, discards failures, learns across iterations. Inspired by Karpathy's autoresearch and codex-autoresearch. Works with any agent (Codex, Claude Code, subagents) and any language or domain.
---

# Autoresearch Loop

You are running an autonomous improvement loop. The goal is measurable. Each iteration makes one atomic change, verifies it, and keeps or discards the result. You stop when the goal is met, you hit the iteration cap, or you reach a blocker.

## Core Loop

```
1. Read context + lessons file
2. Pick ONE hypothesis
3. Make ONE atomic change
4. Commit (before verification)
5. Run VERIFY — did the target metric improve?
6. Run GUARD — did anything else break?
7. Decision: keep / discard / rework
8. Log the result
9. Health check (3+ discards? escalate)
10. Repeat
```

Read `references/loop-protocol.md` for the full loop spec.
Read `references/pivot-protocol.md` for the escalation ladder.
Read `references/lessons-protocol.md` for cross-run learning.

## Before Starting

Confirm with the user:
- **Goal** — one sentence describing what you want to achieve
- **Metric** — what number are you measuring (lower/higher = better)
- **Verify command** — how to measure the metric mechanically
- **Guard command** — what must not break (optional but recommended)
- **Scope** — which files/directories are in play
- **Run mode** — foreground (current session) or background (unattended)
- **Iteration cap** — unlimited, or stop at N

Show what you found and ask for confirmation. One round minimum. Then say "go" to start.

## Verify vs Guard

- **Verify** = "Did the target metric improve?" — measures progress
- **Guard** = "Did anything else break?" — prevents regressions
- Guard files are never modified
- If verify passes but guard fails: rework up to 2 attempts, then discard

## Decision Rules

| Result | Action |
|--------|--------|
| Verify pass + Guard pass | Keep. Extract lesson. |
| Verify pass + Guard fail | Rework (max 2 attempts). If still failing, discard. |
| Verify fail | Discard. Revert. |
| Crash | Auto-fix attempt. If unfixable, skip. |
| Syntax error | Fix immediately. Does not count as iteration. |

## Escalation Ladder

See `references/pivot-protocol.md` for full details.

| Trigger | Action |
|---------|--------|
| 3 consecutive discards | REFINE — adjust within current strategy |
| 5 consecutive discards | PIVOT — abandon strategy, try fundamentally different approach |
| 2 PIVOTs without improvement | Web search for external solutions |
| 3 PIVOTs without improvement | Soft blocker — stop and report to human |

A single successful keep resets all counters.

## Long Run Hygiene

- Every completed experiment must be recorded before the next one starts
- Re-read original instructions every 10 iterations to prevent context drift
- Log: one row per iteration (iteration, commit, metric, delta, status, description)

## Lessons

Extract structured lessons after:
- Every kept iteration (what worked and why)
- Every PIVOT decision (what failed and why)
- Run completion

Store in `autoresearch-lessons.md` (not committed). Consult at the start of each run. Keep ~50 entries, summarise older ones with time decay.
