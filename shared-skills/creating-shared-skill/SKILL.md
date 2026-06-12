---
name: creating-shared-skill
version: 1.0.0
description: "Use when creating a new Hermes skill that should be available to all agent profiles (leo, maya, quinn, rex, and default/iris). Covers skill creation, symlinking into _shared/, and publishing to busycow-playbooks."
triggers:
  - "create a shared skill"
  - "make this skill available to all agents"
  - "add skill to all profiles"
  - "shared skill"
  - "publish skill to playbooks"
---

# Creating a Shared Skill

A shared skill lives in one place and all agent profiles symlink to it.
Never copy — symlinks only. One update propagates to all profiles instantly.

## Directory Layout

```
/mnt/disks/data/hermes/skills/<category>/<skill-name>/   ← single source
        ↑ symlink                    ↑ symlink
~/.hermes/profiles/maya/skills/_shared/<skill-name>
~/.hermes/profiles/leo/skills/_shared/<skill-name>
~/.hermes/profiles/quinn/skills/_shared/<skill-name>
~/.hermes/profiles/rex/skills/_shared/<skill-name>
```

## Step 1 — Create the skill (default profile)

Use `skill_manage(action='create')`. Place it in the appropriate category:

| Category | Use for |
|---|---|
| `core` | Platform tools, GitHub, GBrain, Lark, infrastructure |
| `internal-ops` | Task management, briefings, planning |
| `sales` | CRM, pipeline, quotations, partnerships |
| `knowledge-ops` | GBrain, knowledge capture, extraction |
| `devops` | Deployment, cron, tunnels, Docker |
| `content` | Blog, social, decks, writing |

```python
skill_manage(
    action='create',
    name='<skill-name>',          # gerund form: doing-something
    category='<category>',
    content='...'                 # full SKILL.md content
)
```

This creates the skill at `/mnt/disks/data/hermes/skills/<category>/<skill-name>/`.

## Step 2 — Symlink into all agent profiles

```bash
SKILL_SRC="/mnt/disks/data/hermes/skills/<category>/<skill-name>"
for agent in leo maya quinn rex; do
  ln -sf "$SKILL_SRC" ~/.hermes/profiles/$agent/skills/_shared/<skill-name>
  echo "$agent ✓"
done
```

Verify one of them:
```bash
ls -la ~/.hermes/profiles/maya/skills/_shared/<skill-name>
# Expected: -> /mnt/disks/data/hermes/skills/<category>/<skill-name>
```

## Step 3 — Publish to busycow-playbooks

```bash
mkdir -p ~/busycow-playbooks/shared-skills/<skill-name>
cp /mnt/disks/data/hermes/skills/<category>/<skill-name>/SKILL.md \
   ~/busycow-playbooks/shared-skills/<skill-name>/SKILL.md

# If the skill has reference files or scripts, copy those too:
# cp -r /mnt/disks/data/hermes/skills/<category>/<skill-name>/references/ \
#        ~/busycow-playbooks/shared-skills/<skill-name>/references/
```

**Before pushing, universalize:**
- Replace any hardcoded IDs (Lark table IDs, open_ids, app tokens) with `{{PLACEHOLDER}}`
- Remove company-specific names (DataXquad, AquaOptima, etc.) → `[Company]`
- Remove personal paths (`/home/hunter_lin`) → `~`

```bash
cd ~/busycow-playbooks
git add -A
git commit -m "feat: add shared skill <skill-name>"
git push origin main

# Sync to GBrain so all agents can discover it
gbrain sync --repo ~/busycow-playbooks
```

## Step 4 — Update Skills Registry in Lark Base

The Skills Registry (Lark Base `OircbPodaawVZlsQP2vjThkQp6b`) tracks all skills.
Add a row: Name / Description / Category / Source=shared.

## Pitfalls

- **Never copy the skill folder into profiles** — only symlink. Copies go stale when the skill is updated.
- **`_shared/` takes a symlink directly to the skill folder**, not to a category subdirectory. The path is `_shared/<skill-name>` not `_shared/<category>/<skill-name>`.
- **Universalize before pushing to playbooks** — the playbook is the install source for other teams; internal IDs must not leak.
- **Run `gbrain sync` after push** — otherwise agents querying GBrain won't see the new skill until the nightly cron runs.
