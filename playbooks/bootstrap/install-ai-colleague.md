# Install AI Colleague

> Audience: Default Hermes.
> Scope: install one selected role-owning AI colleague after the core infrastructure has been verified.

Run this only after `playbooks/bootstrap/install-core-infrastructure.md` has completed or after a human owner has accepted remaining core gaps.

---

## 0. Stop Conditions

Stop before installing or activating a colleague if:

- no approved design spec exists and the human owner has not accepted draft-only install
- no target Hermes profile name is defined
- required credentials are missing
- required context sources are missing with no fallback
- structured system writes are required but no write authority or gateway exists
- external publishing or messaging is requested without approval rules
- no human owner is known

---

## 1. Select Colleague Artifact

Expected location:

```text
artifacts/agents/{{agent_slug}}/
```

Expected shape:

```text
artifacts/agents/{{agent_slug}}/
  design-spec.md
  build-blueprint.md
  runtime-artifacts.md
  profile/
    SOUL.md
    config.yaml.template
    cron/
  workspace/
    AGENTS.md
    role-context.md
    authority.md
    tool-policy.md
    memory-policy.md
    routines.md
    evaluation-policy.md
  skills/
```

If this structure does not exist, create a gap report. Do not invent production status.

---

## 2. Review Seven-Layer Design

Confirm decisions exist for:

| Layer | Required decision |
|---|---|
| Identity | role, owner, responsibilities, non-responsibilities |
| Context | GBrain, Hindsight, structured systems, workspace, source priority |
| Capability | skills, tools, outputs, inputs |
| Authority | allowed actions, approval-required actions, forbidden actions |
| Autonomy | routines, schedules, stop conditions |
| Evaluation | quality checks, tests, reviewer |
| Governance | logs, audit, change management, escalation |

If a high-risk decision is missing, install as Draft only or stop.

---

## 3. Create Hermes Profile

```bash
hermes profile create {{profile_name}}
hermes profile use {{profile_name}}
```

Install profile artifacts:

```bash
cp artifacts/agents/{{agent_slug}}/profile/SOUL.md ~/.hermes/profiles/{{profile_name}}/SOUL.md
cp artifacts/agents/{{agent_slug}}/profile/config.yaml.template ~/.hermes/profiles/{{profile_name}}/config.yaml
```

Do not install cron templates until dry run and approval rules are complete.

---

## 4. Create Agent Workspace

```bash
mkdir -p /srv/ai-colleagues/workspaces/{{profile_name}}/{drafts,notes,scratch,examples,runbooks,review-queues}
cp artifacts/agents/{{agent_slug}}/workspace/*.md /srv/ai-colleagues/workspaces/{{profile_name}}/
```

Required docs:

- `AGENTS.md`
- `role-context.md`
- `authority.md`
- `tool-policy.md`
- `memory-policy.md`
- `routines.md`
- `evaluation-policy.md`

If any are missing, mark the colleague as Draft only.

---

## 5. Install Skills

Install mandatory shared skills first, then role-specific skills.

```text
guidelines/05-mandatory-skills.md
```

Copy only skills required by the design spec or build blueprint:

```bash
cp -r artifacts/shared-skills/{{skill_name}} ~/.hermes/profiles/{{profile_name}}/skills/{{skill_name}}
cp -r artifacts/agents/{{agent_slug}}/skills/{{skill_name}} ~/.hermes/profiles/{{profile_name}}/skills/{{skill_name}}
```

Verify:

```bash
hermes skills list
```

---

## 6. Configure Context

Confirm:

- required GBrain canonical sources are readable
- required GBrain evidence zones exist or are marked missing
- personal Hindsight bank exists for this profile
- shared/domain Hindsight writes are governed
- structured systems can be read
- write tools are allowed only where authority permits
- approval-required writes use gateway or human approval
- workspace docs state read-router and write-router rules

---

## 7. Credentials

Do not invent or commit credentials.

Use the target environment's approved secret process.

Confirm required environment variables exist without printing secret values.

---

## 8. Verification

Run:

```bash
hermes doctor
hermes profile use {{profile_name}}
hermes skills list
```

Then test:

- GBrain canonical retrieval
- GBrain evidence retrieval where relevant
- Hindsight recall
- Hindsight personal retain, if enabled
- structured read access
- structured write dry run or approval flow
- Lark/Feishu delivery, if required
- one role-specific skill dry run
- one routine dry run, if routines exist
- evaluation path
- audit/log output

---

## 9. Activation Decision

Use these statuses:

| Status | Meaning |
|---|---|
| Draft | artifacts copied but missing decisions or credentials |
| Testing | profile works but still needs verification |
| Active | verification passed and human owner approved |
| Paused | installed but routines/actions disabled |

A colleague may be marked Active only when:

- human owner is known
- authority policy exists
- required credentials exist
- required context sources work
- required skills pass dry run
- logs are written
- evaluation path exists
- high-risk actions have approval rules

---

## 10. Output Report

Default Hermes should produce:

```text
Agent: {{agent_name}}
Profile: {{profile_name}}
Status: Draft / Testing / Active / Paused
Installed artifacts:
Missing artifacts:
Missing credentials:
Missing tools:
Missing data sources:
Missing approval rules:
Verification results:
Human decisions needed:
Recommended next step:
```