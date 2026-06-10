# Iris — Chief of Staff: Tools

## Overview

Tools Iris reads from and writes to. Iris has the broadest tool access of any agent — she needs full visibility to coordinate the whole company.

---

## Core Tools

| Tool | Purpose | Required | Notes |
|------|---------|----------|-------|
| **Lark Base** | Task Board — source of truth for all task coordination | ✅ Required | App Token + Table IDs must be set. See `SCHEMA.md`. |
| **GBrain** | Company knowledge graph — intel, decisions, distilled outputs | ✅ Required | Full read + write. Must be running. |
| **Lark IM** | Delivering briefings, escalations, and updates to founders | ✅ Required | Configured via `hermes setup lark`. |
| **Hermes Cron** | Scheduling automated maintenance and briefing jobs | ✅ Required | 4 cron jobs created in `SETUP.md` Step 4. |

## Optional Tools

| Tool | Purpose | Capability | Notes |
|------|---------|-----------|-------|
| **Lark Docs / Drive** | Archiving briefing reports and audit outputs | C5, C6 | Useful for long-term record-keeping. |
| **Web Search** | Supplementary research when distilling intel | C7 | Only for strategic planning support tasks. |

---

## Tool Access by Capability

| Capability | Lark Base | GBrain | Lark IM | Hermes Cron |
|------------|-----------|--------|---------|-------------|
| C1 Morning Task Board Review | ✅ R/W | ✅ Read | — | ✅ |
| C2 Agent Output Review & Distillation | ✅ R/W | ✅ R/W | — | — |
| C3 Blocker Detection & Escalation | ✅ Read | ✅ Read | ✅ | — |
| C4 GBrain Maintenance | — | ✅ R/W | ✅ | ✅ |
| C5 Daily Briefing Generation | ✅ Read | ✅ Read | ✅ | ✅ |
| C6 Weekly Task Audit | ✅ Read | — | ✅ | ✅ |
| C7 Strategic Planning Support | ✅ Read | ✅ R/W | ✅ | — |

---

## MCP Servers

| Server | Purpose | Config |
|--------|---------|--------|
| `gbrain` | GBrain knowledge graph access | Configured via `hermes mcp list` |

---

## Configuration Checklist

Before running Iris for the first time:

- [ ] Lark Base app token recorded; Tasks table ID set
- [ ] All `{{TABLE_ID_*}}` placeholders replaced in skills
- [ ] GBrain running — `gbrain status` returns healthy
- [ ] GBrain MCP configured — `hermes mcp list` shows gbrain
- [ ] Lark IM configured — founder chat IDs set in delivery config
- [ ] GitHub brain sync repo configured in `.env`
- [ ] All 4 cron jobs created (see `SETUP.md` Step 4)
- [ ] All operating agent profiles active — `hermes profile list`
