# Tiered Bot Awareness

## Problem

@chuggies_bot runs on Dev Pi via OpenClaw, reads memory banks and issues across all repos, and delivers cross-project status via Telegram. But no repo other than openclaw knows it exists. Claude Code sessions in other repos can't contextualize the bot's activity, and humans have to explain what "tell the bot" means from scratch every time.

## Design

Two tiers of awareness, distributed through CLAUDE.md and memory banks.

### Tier 1 — Low Awareness

**Repos:** All repos except chuggies, chungus-net, ChuggiesMart, openclaw.

**Mechanism:** A block in CLAUDE.md, injected by ChuggiesMart's `/bootstrap-project` skill for new projects and added manually to existing ones.

**Content:**

```markdown
## Chuggies Bot

@chuggies_bot is a Telegram-based AI assistant that reads memory banks and issues across repos. It runs on Dev Pi (192.168.3.4) via OpenClaw. Your memory bank handoffs are consumed by the bot's nightly sweep — keep active-context.md structured and current.
```

**Purpose:**
- Sessions recognize the bot when it appears in issues, commits, or messages
- Sessions understand that memory bank handoffs have a downstream consumer
- Humans can say "tell the bot about this" and the session has enough context to know what that means

### Tier 2 — High Awareness

**Repos:** chuggies, chungus-net, ChuggiesMart, openclaw.

**Mechanism:** Same CLAUDE.md block as Tier 1, plus a section in `tech-context.md`.

**tech-context.md section content:**

```markdown
## Chuggies Bot (@chuggies_bot)

Telegram-based AI assistant providing cross-project awareness across the Chuggies ecosystem.

| Attribute | Value |
|-----------|-------|
| Host | Dev Pi (192.168.3.4), user chris |
| Platform | OpenClaw 2026.3.x |
| Service | openclaw-gateway.service (systemd) |
| Model | anthropic/claude-haiku-4-5 |
| Config | ~/.openclaw/openclaw.json |
| Workspace | ~/.openclaw/workspace/ |

### Interfaces

- **Telegram**: @chuggies_bot (polling, no inbound ports)
- **CLI**: `openclaw message send`, `openclaw agent --message "..."` (from dev-pi)
- **Gateway**: WebSocket on port 18789 (local only)

### What it reads

- Memory banks: `{repo}/.claude/memory-bank/active-context.md` for all dev projects
- Issues: `gh issue list --repo chuggies510/{repo}` via exec/bash
- HA state: REST API at 192.168.3.3:8123

### Cron jobs

| Job | Schedule | Purpose |
|-----|----------|---------|
| daily-digest | 8am Pacific | Cross-project status to Telegram |
| refresh-memory | 2am Pacific | Rewrites MEMORY.md from all memory banks |

### Skill ownership

ChuggiesMart owns skill definitions that the bot uses. Bot workspace files (SOUL.md, IDENTITY.md, TOOLS.md, etc.) live on dev-pi at ~/.openclaw/workspace/.

### Troubleshooting

- Logs: `journalctl -u openclaw-gateway.service -f`
- Restart: `sudo systemctl restart openclaw-gateway.service`
- Config: `~/.openclaw/openclaw.json`
- API key: `~/.openclaw/.env` (chmod 600)
```

### Bootstrap Integration

ChuggiesMart's `/bootstrap-project` skill adds the Tier 1 CLAUDE.md block to new projects automatically. High-awareness repos (3-4 total) are maintained manually since they rarely change.

## Scope

### In scope
- CLAUDE.md blocks for all repos (Tier 1)
- tech-context.md sections for high-awareness repos (Tier 2)
- Bootstrap skill update in ChuggiesMart

### Out of scope
- Memory bank format changes (current active-context.md structure is sufficient)
- `/stop` skill changes (handoff fields already match bot consumption needs)
- Inter-repo communication mechanisms (future work, after build-vs-adopt decision)
- Bot-side changes (no OpenClaw config or workspace file modifications)

## Implementation Sequence

1. Add CLAUDE.md block to existing low-awareness repos
2. Add CLAUDE.md block + tech-context.md section to high-awareness repos (chuggies, chungus-net, ChuggiesMart)
3. Update ChuggiesMart `/bootstrap-project` skill to inject Tier 1 block

## Risks

| Risk | Mitigation |
|------|------------|
| Bot architecture changes after build-vs-adopt decision | CLAUDE.md block is generic enough to survive. tech-context.md sections get updated if platform changes. |
| Stale awareness info | tech-context.md is loaded at session start and reviewed during memory audits |
