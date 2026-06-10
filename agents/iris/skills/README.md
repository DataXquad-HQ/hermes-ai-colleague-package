# Iris Skills

Skills used by Iris are installed from the main Hermes skills directory (`~/.hermes/skills/internal-ops/`), not bundled here.

## Skills List

| Skill | Category | Purpose |
|-------|----------|---------|
| `managing-tasks` | internal-ops | Create and update tasks in Task Board |
| `reviewing-tasks` | internal-ops | Query and summarise task board status |
| `auditing-tasks` | internal-ops | Weekly task structure audit |
| `generating-task-briefing` | internal-ops | Generate daily task briefing for founders |
| `planning-next-actions` | internal-ops | Plan next actions from task board state |
| `extracting-lark-to-gbrain` | knowledge-ops | Extract Lark messages → GBrain |
| `maintaining-gbrain` | knowledge-ops | Nightly GBrain dream cycle |
| `capturing-to-gbrain` | knowledge-ops | Capture intel from conversation → GBrain |

These skills live in the main `hermes-agent` skills registry and are referenced by name in Iris's cron jobs and SOUL.md.
