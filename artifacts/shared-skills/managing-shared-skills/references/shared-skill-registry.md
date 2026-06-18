# Shared Skill Registry

**Last Updated:** 2026-06-18
**Canonical Owner:** Iris

This registry defines how DataXquad / BusyCow shared skills are classified.

---

## Class 1 — Shared Canonical Core

These skills are governed centrally by Iris and are safe to copy into multiple active agent profiles.
They define common operating rules rather than one agent's private workflow.

| Skill | Canonical path | Current rollout | Notes |
|---|---|---|---|
| `skill-creator` | `core/skill-creator/` | leo, maya, rex | authoritative skill-building guide |
| `managing-skills` | `core/managing-skills/` | leo, maya, rex | governs skill lifecycle |
| `managing-shared-skills` | `core/managing-shared-skills/` | leo, maya, rex | governs shared-skill rollout |
| `capturing-to-gbrain` | `core/capturing-to-gbrain/` | leo, maya, rex | durable knowledge capture |
| `routing-report-delivery` | `core/routing-report-delivery/` | leo, maya, rex | separates human report vs cron receipt |

### Rule
If one of these changes and the change is intended to propagate everywhere,
update the canonical source first, then re-copy into the active profiles.

---

## Class 2 — Shared Infra / Selective Install

These skills have a canonical source but should only be installed into profiles that actually need them.

| Skill | Why selective |
|---|---|
| `reading-lark-files` | only agents that routinely inspect shared files need it |
| `twenty-crm` | CRM infra, not every agent uses it at runtime |
| `openmail` | mail infra, primarily Leo / possibly Iris |
| `google-workspace` | only profiles doing Google operations need it |
| `lark-mcp-setup` | setup / repair skill, not normal runtime baseline |
| `managing-lark-cli-identity` | identity governance, not every profile needs it |
| `operating-lark-cli-identity` | operating playbook, not every profile needs it |
| `managing-tasks` | internal ops, only where task operations are part of the role |
| `reviewing-tasks` | internal ops, selective |
| `planning-next-actions` | mainly operator / planning contexts |
| `auditing-tasks` | audit workflow, selective |
| `generating-task-briefing` | reporting workflow, selective |
| `generating-daily-ops-briefing` | ops reporting, selective |
| `extracting-lark-to-gbrain` | Iris / knowledge-ops route |
| `ingesting-sessions-to-hindsight` | Iris / knowledge-ops route |
| `syncing-brain-memory` | Iris / knowledge-ops route |
| `maintaining-gbrain` | Iris / knowledge maintenance |
| `maintaining-memory` | governance / architecture |
| `checking-context-health` | Iris / system ops |

---

## Class 3 — Iris-only Operating Skills

These are canonical but should normally remain in Iris's operating layer rather than being rolled out broadly.

| Skill | Reason |
|---|---|
| `packaging-to-github` | external package publication route |
| `operating-dx-gbrain-vault` | internal durable knowledge route |
| `governing-agent-identity` | persona / identity governance |

---

## Class 4 — Agent-local Skills

These should remain owned by the agent domain unless reuse proves otherwise.

### Leo-local
All CRM / outbound / pipeline workflow skills under `profiles/leo/skills/crm/`.

### Rex-local
Support / maintenance workflows such as `maintenance-project-management` and `remote-debug`.

### Maya-local / transitional
`agent-soul-framework` remains local until deliberately merged into another canonical governance skill.

---

## Class 5 — Transitional / To Remove / To Split

| Skill | Status |
|---|---|
| `github-core-repos` | delete; logic split between `packaging-to-github` and `operating-dx-gbrain-vault` |

---

## Operating Principle

- external reusable artifact → `packaging-to-github`
- internal durable knowledge → `operating-dx-gbrain-vault`
- agent-private workflow → keep local
- common governance / capture rule → shared canonical core
