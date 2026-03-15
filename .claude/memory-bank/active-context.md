---
session: 1
---

# OpenClaw Active Context

## Current Work Focus

### Current State
- **OpenClaw 2026.3.13** installed on Dev Pi (`/usr/local/lib/node_modules/openclaw`)
- **`@chuggies_bot`** on Telegram — responding, Haiku 4.5 model
- **`openclaw-gateway.service`** running as system-level systemd service — survives reboots
- Config: `~/.openclaw/openclaw.json` on Dev Pi
- Tools: read-only (write/edit/exec/bash/process/apply_patch denied)
- Model: `anthropic/claude-haiku-4-5`
- ANTHROPIC_API_KEY persisted in `~/.bashrc` on Dev Pi
- Brave Search enabled (api key in config)
- Hooks: command-logger, session-memory

### Remaining Spike Work
- Write BOOT.md — tell the bot who it is and what it knows
- Custom skill: cross-project status (reads memory banks, pulls `gh issue list` across repos)
- Proactive cron: daily digest pushed to Telegram
- Evaluate: OpenClaw vs handrolled Bun bot decision

---

## CONTEXT HANDOFF - 2026-03-15 (Session 1)

### Session Summary
Full OpenClaw spike session. Brainstormed problem space, decided to evaluate OpenClaw properly before building handrolled alternative. Installed on Dev Pi, connected Telegram, got bot responding, created system service, locked to read-only.

**Chat**: S61-openclaw-spike-install

### What was done

| Task | Status |
|------|--------|
| Brainstorm: problem space, OpenClaw vs handrolled | Done |
| Bootstrap openclaw project | Done |
| Research OpenClaw install process | Done |
| Check Dev Pi prerequisites | Done — Node 24, cmake installed |
| `npm install -g openclaw@latest` | Done |
| `openclaw onboard` — Telegram, Anthropic, Brave Search, hooks | Done |
| Pairing approve own Telegram user (8218488161) | Done |
| Fix API key (cached bad key in auth-profiles.json) | Done |
| Create `/etc/systemd/system/openclaw-gateway.service` | Done |
| Lock tools to read-only | Done |
| Switch model to Haiku 4.5 | Done |

### Key Findings

| Finding | Detail |
|---------|--------|
| DietPi masks systemd-logind intentionally | Physical hardware — not needed. VMs/desktops only. |
| User-level systemd unavailable | logind masked → no `XDG_RUNTIME_DIR`. OpenClaw detects and skips gracefully. |
| System-level service is correct pattern | Matches all other services on Dev Pi |
| OpenClaw caches API key in `~/.openclaw/agents/main/agent/auth-profiles.json` | Changing env var alone doesn't fix billing errors — must update this file |
| `agents.defaults.model.primary` is the model config path | Not `agents.main.model` — wrong path crashes gateway in a loop |
| Telegram bot token: `8481693506:AAEqW-...` | In session history — rotate via BotFather |
| Gateway token: `a2cafcb6ace60c21...` | In session history — rotate via `openclaw configure` |

### Gotchas discovered
- OpenClaw's `--install-daemon` skips silently when user systemd unavailable — create system service manually
- API key is cached in `auth-profiles.json`, not just read from env at runtime
- `agents.main.model` is an unrecognized key — use `agents.defaults.model.primary`
- cmake needed for native module builds (sqlite-vec) — install before npm global install

### Next Session Priority
1. Rotate bot token (BotFather `/revoke`) and gateway token (`openclaw configure`)
2. Write BOOT.md for bot identity and ecosystem context
3. Build cross-project status skill

