# Iris — Chief of Staff: Schema

## Overview

Iris primarily reads and writes the **Task Board** in Lark Base. This is the single coordination surface shared across all agents.

---

## Task Board Schema

### Tasks Table

| Field | Type | Purpose |
|-------|------|---------|
| Task Title | Text (Primary) | Short description of the work |
| Assigned To | Single Select | Which agent owns this task |
| Status | Single Select | `To Do / In Progress / Blocked / Done` |
| Priority | Single Select | `🔴 High / 🟡 Medium / 🟢 Low` |
| Due Date | Date | When the task must be complete |
| Related Agent | Single Select | `Iris / Leo / Maya / Quinn / Rex / Steve` |
| Agent Notes | Long Text | Written by the executing agent after completion |
| Result for Human | Long Text | Written by Iris — plain-language output for founders |
| Done | Checkbox | Set true when complete |
| Blocked Reason | Long Text | Why the task is blocked (if applicable) |
| Dependencies | Text | Other task IDs this task depends on |
| Handoff Context | Long Text | Injected by Iris when reassigning |
| Sprint | Single Select | Current sprint label (e.g. `2026-Q2-S3`) |
| Project | Single Select | Which project this belongs to |

### Field Write Rules

| Field | Who Writes |
|-------|-----------|
| Task Title, Priority, Due Date | Iris (on creation) |
| Assigned To | Iris |
| Agent Notes | Executing agent |
| Result for Human | Iris only |
| Done | Executing agent |
| Status | Any agent |
| Handoff Context | Iris only |
| Blocked Reason | Any agent |

---

## Three-Layer Memory Model

| Layer | Tool | Stores |
|-------|------|--------|
| **Live Task State** | Lark Base | Current status, assignments, outputs |
| **Accumulated Knowledge** | GBrain | Decisions, intel, distilled agent outputs |
| **Documents** | Lark Docs/Drive | Briefing archives, audit reports |

---

## GBrain Page Conventions

### Decisions
```
decisions/YYYY-MM-DD-topic-slug
---
type: decision
date: YYYY-MM-DD
made_by: Hunter / Kevin / Iris
status: active
---
# [Decision Title]
## Context
## Decision
## Rationale
## Implications
```

### Agent Status Pages
```
agents/[agent-name]
---
type: agent
role: Chief of Staff / CRO / etc.
status: active
---
# [Agent Name]
## Current Focus
## Recent Outputs
## Known Blockers
```

---

## Template Variables

| Placeholder | Value |
|-------------|-------|
| `{{LARK_APP_TOKEN}}` | Task Board Bitable App Token |
| `{{TABLE_ID_TASKS}}` | Tasks table ID |
| `{{TABLE_ID_ACCOUNTS}}` | Accounts table ID |
| `{{TABLE_ID_CONTACTS}}` | Contacts table ID |
| `{{TABLE_ID_OPPORTUNITIES}}` | Opportunities table ID |
| `{{TABLE_ID_PARTNERSHIPS}}` | Partnerships table ID |
