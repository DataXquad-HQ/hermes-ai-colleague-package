# Agent Playbooks

Each subdirectory is a self-contained agent — everything needed to install, configure, and run that agent on a new workspace.

## Available Agents

| Agent | Role | Status |
|-------|------|--------|
| [leo](./leo/) | BD Director — pipeline progression, account intelligence, partner management | ✅ Active |

## Structure

Each agent directory contains:

```
agents/<name>/
├── CAPABILITY.md   ← What the agent does, C1–CN capabilities, authority grid
├── SOUL.md         ← Agent identity, operating principles, tone
├── SCHEMA.md       ← CRM / database schema the agent reads and writes
├── SETUP.md        ← Step-by-step install guide (agent-executable)
└── skills/         ← All skills the agent uses
    ├── skill-name.md
    └── ...
```

## How to Install an Agent

1. Read `agents/<name>/SETUP.md`
2. Follow the steps — the guide is written to be executed by an agent (or a human)
3. Replace all `{{PLACEHOLDER}}` values with your actual IDs and tokens
4. Run the verification steps at the end

## Adding a New Agent

1. Create `agents/<name>/` directory
2. Write `CAPABILITY.md` — define the role, capabilities, and authority grid
3. Write `SOUL.md` — define identity and operating principles
4. Write `SCHEMA.md` — define the data structures the agent needs
5. Write `SETUP.md` — step-by-step install instructions
6. Add skills to `skills/`
