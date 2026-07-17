# Blueprint Skill

Methodology plugin for Claude Code. Battle-tested on 5+ production projects.

Not a tool — a **way of thinking**: challenge before choosing, plan before touching code,
prove before claiming. Works on any project, any stack.

## Philosophy

> "Le cerveau planifie, les mains executent."
> **All the intelligence lives in the plan. Execution is mechanical.**
> If a junior fails to execute your plan, the plan was bad — not the junior.

Corollary that drives everything: **a perfect plan built on a misunderstood goal is worthless.**
Precision never rescues a wrong objective. Hence: resolve ambiguity *first*, always.

## Install

```bash
claude plugin marketplace add https://github.com/Romgaal/blueprint-skill
claude plugin install blueprint-skill@romain-methods
```

> ⚠️ After editing a skill, **reinstall the plugin** — Claude Code serves a cached copy,
> so editing the source alone is not enough.

## The workflow

```
  crash-test            audit an existing thing → findings + 3 solutions each
      ↓
  /blueprint
      ├── blueprint-plan
      │     Phase 0    capture requirements verbatim (compaction destroys the rest)
      │     Phase 0.5  LIFT AMBIGUITIES — ask what wasn't said, verify unknowns, list decisions
      │     Phase 1    → invokes challenge-assumptions (3+ options, best one wins)
      │     Phase 2    the CDC: every step copy-pasteable + global success criterion
      ├── blueprint-execute    mechanical, step by step, zero improvisation
      ├── verification-proof   prove it, "should" is banned
      └── blueprint-reviewer   reviews the implementation AND the quality of the plan itself
```

## Skills

| Skill | Trigger |
|---|---|
| **blueprint-plan** | A task spanning several files or >30 min. Lifts ambiguities, delegates the approach choice, produces the CDC. |
| **challenge-assumptions** | There is a technical **choice** to make. 3 options minimum, biases killed, best one justified. |
| **blueprint-execute** | A validated blueprint exists. Execute it mechanically, report every deviation. |
| **verification-proof** | About to claim something is done. Turn the claim into proof — run it, show the output. |
| **systematic-debug** | Something already built **fails**. 4 phases: logs → config → source → experiment. Never guess. |
| **crash-test** | Audit an **existing** product/UI/system: frictions, UX, security, perf, debt → 3 distinct solutions per finding. |

## Agents

- **blueprint-reviewer** — Compares the work to the blueprint, detects every deviation, and
  judges whether each gap was an *execution fault* or a **plan fault**. Plan faults feed the gotchas.

## Commands

- **/blueprint** — Full workflow: plan → execute → verify → review

## References

- **`references/gotchas.md`** — real planning mistakes, dated, with what should have been done.
  Loaded on demand (progressive disclosure). **Grow it after every failed plan** — it is the
  highest-signal content of this plugin.

## Origin

Forged from the SOUL.md of a personal assistant bot and the CLAUDE.md of a production VPS running
20+ Docker services. Every rule exists because we learned it the hard way — and each gotcha is a
scar, not a theory.
