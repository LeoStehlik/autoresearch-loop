# autoresearch-loop

An autonomous modify-verify-decide loop for any agent and any measurable goal.

You give it a goal and a metric. It iterates — making one atomic change at a time, verifying, keeping improvements, discarding failures — until the goal is met or you stop it. It learns across iterations and knows when to escalate.

---

## What it does

- Runs a tight modify-verify-keep/discard loop toward a measurable target
- Separates **Verify** (did the metric improve?) from **Guard** (did anything else break?)
- Escalates intelligently: REFINE after 3 failures, PIVOT after 5, web search after 2 pivots, soft blocker after 3 pivots
- Extracts lessons after every iteration and uses them to bias future hypotheses
- Prevents context drift on long runs by re-reading instructions every 10 iterations
- Works for anything with a number: test coverage, type errors, lint warnings, performance, research quality, translation completeness

---

## Installation

### OpenClaw

Add your workspace skills directory to `openclaw.json`:

```json
{
  "skills": {
    "load": {
      "extraDirs": ["/path/to/your/skills"]
    }
  }
}
```

Clone into that directory:

```bash
git clone https://github.com/LeoStehlik/autoresearch-loop.git /path/to/your/skills/autoresearch-loop
```

### Codex / Claude Code / other agents

Copy `SKILL.md` and the `references/` folder into your project's skills directory (`.agents/skills/autoresearch-loop/`), then invoke with `$autoresearch-loop`.

---

## Usage

Say what you want in one sentence:

```
$autoresearch-loop
Reduce TypeScript `any` types to zero in src/**/*.ts
```

The agent will:
1. Scan and confirm: goal, metric, verify command, guard command, scope
2. Ask: foreground (current session) or background (unattended)?
3. Ask: run until done, or cap at N iterations?
4. Start on "go"

---

## The Loop

```
read state + lessons
pick ONE hypothesis
make ONE atomic change
commit
run VERIFY
run GUARD
keep / discard / rework
log result
check escalation ladder
repeat
```

---

## Escalation Ladder

| Trigger | Action |
|---------|--------|
| 3 consecutive discards | REFINE - adjust within current strategy |
| 5 consecutive discards | PIVOT - fundamentally different approach |
| 2 PIVOTs without improvement | Web search for external solutions |
| 3 PIVOTs without improvement | Soft blocker - stop and report to human |

A single successful keep resets all counters.

---

## What's Inside

```
autoresearch-loop/
├── SKILL.md                          Core loop rules + decision protocol
└── references/
    ├── loop-protocol.md              Full iteration spec + results log format
    ├── pivot-protocol.md             REFINE/PIVOT/soft-blocker escalation
    └── lessons-protocol.md           Cross-run learning + lesson format
```

---

## Inspiration

Inspired by [Karpathy's autoresearch](https://github.com/karpathy/autoresearch) and [codex-autoresearch](https://github.com/leo-lilinxiao/codex-autoresearch). Built as our own clean, agent-agnostic implementation.

---

## License

MIT - see [LICENSE](LICENSE)
