# gsdxsuperpower reference: GSD MCP tools & task-type verification

## 1. Use GSD MCP tools, never hand-edit state

`.gsd/*.md` (STATE.md, SUMMARY.md, PROJECT.md) are human-readable projections of
the GSD database. They can lag or contradict each other. Read and write GSD state
only through these tools:

| Intent | Use this tool | Do NOT |
|---|---|---|
| Load authoritative state / resume | `gsd_resume`, `gsd_milestone_status` | read `STATE.md` to decide work |
| Slice ordering / dependencies | `gsd_graph`, milestone ROADMAP | enumerate folders |
| Existing task breakdown | `gsd_plan_task` records / `T0x-PLAN.md` | re-decompose with writing-plans |
| Complete a task | `gsd_complete_task` | hand-edit STATE.md |
| Complete a slice | `gsd_complete_slice` | hand-edit STATE.md |
| Record a slice summary / evidence | `gsd_save_summary` | hand-write SUMMARY.md |
| Record a gate result | `gsd_save_gate_result` | (lose it) |
| Complete a milestone | `gsd_complete_milestone` | hand-edit STATE.md |
| Record an architectural decision | `gsd_save_decision` | (lose it) |
| Look up prior attempts / history | `gsd_journal_query` | rely on subagent memory |
| Collect missing credentials/secrets | `secure_env_collect` | immediately interrupt the user |

Markdown files remain fine to read for human context; they are never the
authority and are never edited by hand inside this workflow.

## 2. Task-type verification matrix

The verification discipline depends on the task type. Pick the row that matches;
do not force a unit test where it does not belong.

| Task type | Verification discipline |
|---|---|
| Business logic / data layer | Strict TDD: red -> green -> refactor. Implementation before a failing test -> delete it, start over. |
| UI / interaction | Component test where practical, otherwise evidence-based verification (screenshots / observed behavior) via verification-before-completion. |
| Infra / build / config | Build/command succeeds + explicit acceptance command. No contrived unit test required. |
| E2E / integration | Scenario-based acceptance script; evidence over claims. |

When GSD task metadata declares a type, trust it. Otherwise infer from the task's
files and goal. "Config exemption" is not a binary whitelist; it is the
infra/build/config row above.
