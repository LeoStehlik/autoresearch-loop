# Loop Protocol

## Full Loop (per iteration)

1. **Review state** — read git history, results log, lessons, current metric
2. **Pick hypothesis** — one atomic change that could improve the metric. Apply multiple lenses: algorithmic, structural, configuration, dependency, test coverage angle.
3. **Make the change** — one file, one concern. Atomic.
4. **Commit** — before running verification. This enables clean rollback.
5. **Run VERIFY** — execute the verify command. Did the metric improve?
6. **Run GUARD** — execute the guard command. Did anything regress?
7. **Decide** — see decision rules in SKILL.md
8. **Log** — append one row to results log before starting the next iteration
9. **Health check** — check discard streak, apply escalation ladder if needed
10. **Continue** — or stop if terminal condition met

## Terminal Conditions

Stop when ANY of the following is true:
- Goal metric reaches target
- Iteration cap reached
- Soft blocker triggered (3 PIVOTs without improvement)
- Manual stop requested

## What NOT to Do

- Never make more than one logical change per iteration
- Never skip logging before starting the next iteration
- Never modify guard files
- Never pause mid-loop to ask the user (unless it is a soft blocker)
- Never assume a change worked without running verify

## Results Log Format

```
iteration  commit   metric  delta  status   description
0          abc123   47      0      baseline initial state
1          def456   41      -6     keep     fixed auth module types
2          def456   49      +8     discard  generic wrapper made things worse
3          def456   38      -3     keep     type-narrowed API handlers
```

- `status`: baseline / keep / discard / skip / pivot / refine
- Log is not committed — it is an autoresearch-owned artifact
- Print progress summary every 5 iterations
- Print baseline-to-best summary at run completion

## Rollback Strategy

Approved before launch. Default: `git revert --no-edit HEAD`. In isolated branch/worktree: hard reset is acceptable. The results log remains the audit trail regardless.
