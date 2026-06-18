# Iris Skills — Package Status

## What is true right now

Iris already depends on a meaningful **shared skill layer** in Hermes, but the
**Iris package-local deploy layer is not fully built yet**.

That means:
- Iris can already execute many Chief-of-Staff workflows in the live environment
- but the package does **not yet carry all of those skills as explicit Iris deploy artifacts**
- so package completeness and runtime capability are currently different things

---

## Package-local Iris skills currently present

| Skill | Status | Purpose |
|---|---|---|
| `openmail` | present | read Leo's inbox for pipeline / outreach monitoring |
| `capturing-operating-changes` | present | capture structural / operating decisions across GBrain, Hindsight, and task state |

See also: `references/runtime-scenario-coverage.md` for the current scenario test matrix.

---

## Shared skills Iris currently relies on in runtime

These exist in the shared Hermes skill layer and Iris can already use them in the live environment.
They are the capability primitives behind the spec.

### C1 — Operations, Team & Agent Management
- `managing-tasks`
- `reviewing-tasks`
- `planning-next-actions`
- `generating-task-briefing`
- `generating-daily-ops-briefing`

### C2 — Infrastructure Management
- `checking-context-health`
- `managing-cron-jobs`
- `packaging-to-github`
- `managing-skills`

### C3 — Context, Memory & Knowledge Management
- `extracting-lark-to-gbrain`
- `ingesting-sessions-to-hindsight`
- `capturing-to-gbrain`
- `maintaining-gbrain`
- `syncing-brain-memory`
- `managing-team-knowledge`

### General tool-oriented skills Iris frequently needs
- `lark-im`
- `lark-base`

---

## The gap

The gap is **not** that Iris has zero capability.
The gap is that Iris package deploy artifacts do **not yet explicitly bundle or map** the full skill layer she depends on.

In other words:
- **runtime competence:** partly real already
- **package completeness:** still incomplete

---

## Build order for the Iris deploy skill layer

### Wave 1 — Mandatory package-local governance skills
These define Iris as Chief of Staff rather than just a consumer of shared primitives.

1. `capturing-operating-changes` ✅ done
2. `reviewing-operating-state` ⏳ not built
3. `routing-founder-decisions` ⏳ not built
4. `reviewing-agent-output` ⏳ not built

### Wave 2 — Bring shared dependencies into explicit deploy scope
These do not all need Iris-specific rewrites, but the package should explicitly declare or copy them.

1. `managing-tasks`
2. `reviewing-tasks`
3. `planning-next-actions`
4. `generating-task-briefing`
5. `generating-daily-ops-briefing`
6. `checking-context-health`
7. `managing-cron-jobs`
8. `extracting-lark-to-gbrain`
9. `ingesting-sessions-to-hindsight`
10. `capturing-to-gbrain`
11. `maintaining-gbrain`
12. `syncing-brain-memory`
13. `managing-team-knowledge`

### Wave 3 — Optional Iris-specific specialization
Only create these if repeated use shows the generic shared skill is not enough.

- `governing-okr-and-task-state`
- `running-weekly-ops-review`
- `triaging-internal-blockers`
- `auditing-knowledge-coverage`

---

## Scenario coverage — what is already proven vs not yet proven

### Already proven in real runtime
- migrate old operational structure into Lark Tasks / OKR
- refactor OKR based on current company state
- re-map execution tasks to active objectives / KRs
- inspect cron health and context system status
- verify GBrain / Hindsight / session-ingest pipelines are alive

### Partly proven
- using live company documents to change company operating priorities
- deciding what is stale vs active in the internal operations layer
- using hot + cold memory together to support CoS judgment

### Not yet fully proven
- founder makes a decision → Iris updates decision log + active state + Hindsight + task layer in one clean flow every time
- agent delivers work → Iris reviews it and writes a stable “result for human” layer
- customer / partner handoff governance across agents
- incident / blocker escalation playbook
- recurring weekly operating review loop with explicit closure criteria

---

## Practical reading of package status

If you ask "Can Iris do useful Chief-of-Staff work right now?"
- **Yes, partly.**

If you ask "Is Iris package-complete as a deployable Chief-of-Staff agent?"
- **No, not yet.**

The next most important package work is to turn Iris's repeated governance moves
into explicit package-local skills, then make the shared dependencies visible in
her deploy layer.
