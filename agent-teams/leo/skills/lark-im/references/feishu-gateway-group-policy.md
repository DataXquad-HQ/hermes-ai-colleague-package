# Feishu Gateway — Group Policy Config

## Problem

By default, Leo's Hermes gateway only responds to **direct messages (DMs)**.
`@` mentions in group chats are silently dropped — the bot is present in the
group but never replies. No error is raised; messages are simply discarded.

## Root Cause

`feishu.default_group_policy` defaults to empty string, which falls back to
`group_policy` (also unset), resulting in silent discard of all group messages.

## Fix

Add to `~/.hermes/profiles/leo/config.yaml`:

```yaml
feishu:
  default_group_policy: open
  require_mention: true
```

- `default_group_policy: open` — allows any group the bot is in to trigger a response
- `require_mention: true` — only fires when the bot is explicitly `@`-mentioned (not every message)

Then restart the gateway from **outside** the running gateway process:

```bash
hermes --profile leo gateway restart
```

> ⚠️ You cannot restart the gateway from within itself — it refuses with
> "Refusing to restart from inside the gateway process." Always run the
> restart from a separate shell session.

## Verification

After restart, go to the target group and send a message `@Leo [anything]`.
Leo should respond within a few seconds if the config took effect.

## Affected Groups (as of 2026-06-15)

| Group | chat_id | Policy |
|---|---|---|
| `[Sales] Nurturing Outreach Review` | `oc_28f34b34f4da3a13ddc618b19d1c458f` | open, require_mention |
| `[DX] Sales Daily Update` | `oc_a5e03bcb6026a81a5a330b53c4e90575` | open, require_mention |
| `[System] Backend Report` | `oc_8c3706de744958173c700d995ccfd4ef` | open, require_mention |
