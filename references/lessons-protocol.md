# Lessons Protocol

## When to Extract Lessons

| Event | Lesson type |
|-------|-------------|
| Every kept iteration | Positive — what worked and why |
| Every PIVOT decision | Strategic — what failed and why, what to try instead |
| Run completion | Summary — overall findings, best approach found |

## Lesson Format

```markdown
## [type] — [date] — iteration [N]
- **What:** [one sentence describing the change]
- **Result:** metric went from X to Y (delta: Z)
- **Why it worked / failed:** [concise explanation]
- **Reuse:** [when to apply this lesson again, or what to avoid]
```

## Storage

- File: `autoresearch-lessons.md` in the repo root (not committed)
- Read at the start of every run — consult before picking hypotheses
- Capacity: ~50 entries. When over capacity, summarise older entries (>30 days) into a single paragraph, preserving current-run lessons verbatim.

## How to Use Lessons When Picking Hypotheses

Before picking a hypothesis:
1. Check if a similar approach has been tried before
2. If it failed previously — skip it or apply the lesson to do it differently
3. If it succeeded — try related variants first
4. Positive lessons bias toward proven approaches
5. Strategic lessons bias away from known dead ends

## Time Decay

Lessons older than 90 days are summarised and compressed. The summary preserves:
- What general strategies worked
- What general strategies failed
- Any hard constraints discovered (e.g. "guard always fails when X is modified")

Current-run lessons are always kept verbatim regardless of date.
