# Iris — Chief of Staff: Setup Guide

## What Gets Created

| Component | Details |
|-----------|---------|
| **Hermes Profile** | `iris` (default profile in most deployments) |
| **Lark Base Access** | Task Board — Tasks table (R/W), reference tables (Read) |
| **GBrain Access** | Full read + write, all namespaces |
| **Iris Skills** | 8 skills installed |
| **Cron Jobs** | 4 automated jobs |

### 8 Iris Skills
`managing-tasks` · `reviewing-tasks` · `auditing-tasks` · `generating-task-briefing` · `planning-next-actions` · `extracting-lark-to-gbrain` · `maintaining-gbrain` · `capturing-to-gbrain`

---

## Prerequisites

- [ ] Hermes Agent installed and running
- [ ] Lark/Feishu workspace configured (`hermes setup lark`)
- [ ] GBrain running and accessible (`gbrain status`)
- [ ] All operating agent profiles created (leo, maya, quinn, rex, steve)
- [ ] Task Board Lark Base app created (or existing app token available)

---

## Setup Steps

### Step 1 — Configure Lark Base Access

**1a.** Locate or create the Task Board Bitable app → copy App Token from URL

**1b.** Record table IDs:
```
TASKS_TABLE_ID=tbl...
ACCOUNTS_TABLE_ID=tbl...   (read-only reference)
CONTACTS_TABLE_ID=tbl...   (read-only reference)
```

**1c.** Ensure Tasks table has the full field set from `SCHEMA.md`

---

### Step 2 — Install Iris Skills

```bash
SKILLS_BASE="https://raw.githubusercontent.com/DataXquad-HQ/busycow-playbooks/main/agents/iris/skills"
IRIS_SKILLS="~/.hermes/skills/internal-ops"
mkdir -p $IRIS_SKILLS

for skill in managing-tasks reviewing-tasks auditing-tasks generating-task-briefing \
  planning-next-actions extracting-lark-to-gbrain maintaining-gbrain capturing-to-gbrain; do
  mkdir -p "$IRIS_SKILLS/$skill"
  curl -s "$SKILLS_BASE/$skill.md" -o "$IRIS_SKILLS/$skill/SKILL.md"
done
```

**Replace placeholders in all skills:**
```bash
cd ~/.hermes/skills/internal-ops
find . -name "SKILL.md" -exec sed -i 's/{{LARK_APP_TOKEN}}/YOUR_APP_TOKEN/g' {} +
find . -name "SKILL.md" -exec sed -i 's/{{TABLE_ID_TASKS}}/YOUR_TASKS_TABLE_ID/g' {} +
```

---

### Step 3 — Configure GBrain

**3a.** Verify GBrain is healthy:
```bash
gbrain status
```

**3b.** Confirm Hermes has GBrain MCP configured:
```bash
hermes mcp list | grep gbrain
```

**3c.** Configure GitHub sync (if not already set):
```bash
# Set in ~/.hermes/.env or hermes config
GBRAIN_GITHUB_REPO=git@github.com:DataXquad-HQ/brain.git
```

---

### Step 4 — Set Up 4 Cron Jobs

> Schedules in UTC — Taiwan UTC+8: subtract 8 hours

| Job Name | Schedule (UTC) | Purpose |
|----------|---------------|---------|
| `nightly-lark-extract` | `0 19 * * *` | Extract yesterday's Lark messages → GBrain |
| `nightly-gbrain-maintenance` | `0 20 * * *` | Dream cycle + GitHub sync |
| `daily-task-briefing` | `0 1 * * 1-5` | Morning summary to founders (weekdays) |
| `weekly-task-audit` | `0 1 * * 0` | Full task structure audit (Sunday) |

**Example cron command:**
```bash
hermes cron create \
  --name "daily-task-briefing" \
  --schedule "0 1 * * 1-5" \
  --skills "generating-task-briefing" \
  --prompt "Load and follow the generating-task-briefing skill. Generate today's task briefing and deliver to founders."
```

---

### Step 5 — Verify Setup

```bash
# Check Lark access
lark-cli base list-tables --app-token YOUR_APP_TOKEN

# Check GBrain
gbrain status
gbrain search "iris"

# Check cron jobs
hermes cron list

# Run a test briefing
hermes cron run [daily-task-briefing-job-id]
```

---

## SOUL.md Customisation

Edit `~/.hermes/SOUL.md` (or `~/.hermes/profiles/iris/SOUL.md`) and fill in:

- `{{LARK_APP_TOKEN}}` — Task Board app token
- `{{TABLE_ID_TASKS}}` — Tasks table ID
- `{{TABLE_ID_ACCOUNTS}}` — Accounts table ID
- Company name and founder names as appropriate
