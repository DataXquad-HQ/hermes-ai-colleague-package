# Shared Skill Rollout Plan

**Last Updated:** 2026-06-18

This file records the active rollout plan for shared canonical skills.

---

## Shared Core Baseline v1

The following skills are the active shared baseline for current agent profiles:

- `skill-creator`
- `managing-skills`
- `managing-shared-skills`
- `capturing-to-gbrain`
- `routing-report-delivery`

These are expected to exist in:
- `leo`
- `maya`
- `rex`

---

## Completed rollout (2026-06-18)

Canonical-to-profile copies were installed into all active profiles above.

### Verification commands

```bash
for agent in leo maya rex; do
  find ~/.hermes/profiles/$agent/skills -maxdepth 1 -mindepth 1 -type d \
    | sed 's#.*/##' \
    | grep -E '^(skill-creator|managing-skills|managing-shared-skills|capturing-to-gbrain|routing-report-delivery)$'
done
```

---

## Selective rollout candidates

### Leo
- `openmail`
- `twenty-crm`
- `reading-lark-files`

### Maya
- `reading-lark-files` only if file-heavy workflows continue

### Rex
- `reading-lark-files`
- `managing-tasks`
- `reviewing-tasks`
- `managing-team-knowledge`

### Iris only
- `packaging-to-github`
- `operating-dx-gbrain-vault`
- `checking-context-health`
- `maintaining-gbrain`
- `maintaining-memory`
- `extracting-lark-to-gbrain`
- `ingesting-sessions-to-hindsight`
- `syncing-brain-memory`

---

## Rollout rule

Before copying a skill into a profile:
1. confirm the trigger situation truly exists in that role
2. confirm the skill does not hardcode another agent's workflow
3. copy from canonical source, not from another profile copy
4. verify `SKILL.md` loads under the target profile

---

## Decommission rule

When a shared skill is removed or split:
1. patch any SOUL / docs / reference files that still mention it
2. remove runtime copies from active profiles if they are no longer wanted
3. update `shared-skill-registry.md`
4. only then delete the canonical source
