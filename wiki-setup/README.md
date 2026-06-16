# Wiki Setup

This directory contains document templates that must be filled in before the agent team can operate effectively.

These are the **context layer** — they tell agents who you are, who you're selling to, and how you operate. Without them, agents fall back to generic behaviour with no company-specific anchors.

## Required Documents

Copy each template into your `dx-internal-wiki/context/` repo and fill it in.

| Template | Copy to | Primary agent reader | Priority |
|---|---|---|---|
| `company-background.md` | `context/company-background.md` | All agents | First |
| `team.md` | `context/team.md` | All agents | First |
| `product-overview.md` | `context/product-overview.md` | Leo, Maya | First |
| `sales-strategy.md` | `context/sales-strategy.md` | Leo | First |
| `brand-messaging.md` | `context/brand-messaging.md` | Maya, Leo | Before content creation |
| `key-contacts.md` | `context/key-contacts.md` | Leo, Iris | Before outreach |

## Setup Steps

1. Copy all templates into your `dx-internal-wiki/context/` directory
2. Fill in each document — use `[brackets]` as placeholders where you don't have data yet
3. Commit and push
4. Tell Iris: "Extract context from wiki" — Iris will load everything into GBrain
5. Agents are now context-aware

## What happens if you skip a document

| Missing | Impact |
|---|---|
| `company-background.md` | Agents have no company context — generic responses only |
| `team.md` | Agents cannot route escalations or know who decides what |
| `product-overview.md` | Leo cannot qualify leads against what you actually sell |
| `sales-strategy.md` | Leo's Pipeline Health Check has no anchor — cannot flag stalls or mismatches |
| `brand-messaging.md` | Maya's content may not match your brand voice |
| `key-contacts.md` | Leo may treat high-sensitivity contacts incorrectly |

Partial data is better than none. Fill in what you have and update over time.
