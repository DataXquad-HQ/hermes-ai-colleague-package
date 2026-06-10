# Iris — Chief of Staff: Context

> "This document defines everything Iris needs to operate — data sources, GBrain configuration, and documents the human operator must provide."

---

## Overview

Iris operates across **Lark Base (Task Board)**, **GBrain (knowledge graph)**, and **Lark IM**. She reads from all agent workspaces and writes back Results and distilled intel.

---

## 1. Lark Base Data Sources

> All table IDs defined in `SCHEMA.md`

| Table | Access | Purpose |
|-------|--------|---------|
| Tasks | R/W | All tasks across all agents |
| Accounts | Read | Company context for distillation |
| Contacts | Read | People context for distillation |
| Opportunities | Read | Deal context for briefings |
| Partnerships | Read | Partner context for briefings |

### ⚙️ Required Setup Actions
- Create or point to existing Lark Base app (Task Board + Sales & Ops)
- Set `{{LARK_APP_TOKEN}}` and all `{{TABLE_ID_*}}` placeholders in skills

---

## 2. GBrain Configuration

Iris has **full read + write** access across all GBrain namespaces.

### Namespaces Iris Owns

| Namespace | Purpose |
|-----------|---------|
| `decisions/YYYY-MM-DD-topic` | Strategic decisions reached in conversation |
| `agents/` | Agent capability and status pages |
| `concepts/` | Company mental models and frameworks |
| `analysis/` | Synthesis and research output |

### Namespaces Iris Reads

| Namespace | Source Agent |
|-----------|-------------|
| `companies/` | Leo |
| `people/` | Leo |
| `products/` | Quinn |

### Automated GBrain Operations

| Operation | Trigger | Tool |
|-----------|---------|------|
| Create decision page | Key decision in conversation | `mcp_gbrain_put_page` |
| Extract facts from agent output | After reviewing Agent Notes | `mcp_gbrain_extract_facts` |
| Add timeline entry | Company/deal milestone | `mcp_gbrain_add_timeline_entry` |
| Nightly dream cycle | Daily 04:00 UTC+8 | `maintaining-gbrain` skill |
| Sync to GitHub | After dream cycle | `syncing-brain-memory` skill |

### ⚙️ Required Setup Actions
- GBrain instance running (`gbrain status` returns healthy)
- Iris's Hermes profile has GBrain MCP configured
- GitHub repo configured for brain sync

---

## 3. Agent Coordination Model

Iris coordinates five operating agents. Each has a defined escalation path:

| Agent | Role | Escalation Trigger |
|-------|------|-------------------|
| **Maya** | GTM | Content blocked, channel underperforming |
| **Leo** | Revenue & Partnerships | Deal stalled >7 days, partner unresponsive |
| **Quinn** | Product Intelligence | Feedback loop broken, feature request from key client |
| **Rex** | Customer Success | SLA breach, renewal at risk |
| **Steve** | Software Development | Build blocked, architecture decision needed |

### Handoff Context Protocol
When assigning or reassigning a task, Iris injects:
1. Why this task exists (link to decision or goal)
2. What the previous agent did (summary of Agent Notes)
3. What the next agent needs to produce (clear output spec)

---

## 4. Cron Job Schedule

| Job | Schedule (UTC+8) | Skill Triggered | Notes |
|-----|-----------------|----------------|-------|
| `nightly-gbrain-maintenance` | Daily 04:00 | `maintaining-gbrain` + `syncing-brain-memory` | Dream cycle + GitHub sync |
| `nightly-lark-extract` | Daily 03:00 | `extracting-lark-to-gbrain` | Yesterday's Lark messages → GBrain |
| `daily-task-briefing` | Weekdays 09:00 | `generating-task-briefing` | Morning summary to founders |
| `weekly-task-audit` | Sunday 09:00 | `auditing-tasks` | Full task structure audit |

---

## 5. Operator-Provided Inputs

| Input | Where Used |
|-------|-----------|
| Founder Lark IM chat IDs | Delivering briefings and escalations |
| Task Board App Token + Table IDs | All task operations |
| GBrain GitHub repo URL | Brain sync |
| Agent profile names (leo, maya, etc.) | Cross-profile coordination |

---

## Setup Checklist

- [ ] Lark Base app configured; App Token + Task Table ID recorded
- [ ] GBrain running and healthy
- [ ] GitHub repo for brain sync configured
- [ ] Lark IM delivery channel set (founder chat ID)
- [ ] 4 cron jobs created per SETUP.md
- [ ] All agent profiles created and running
