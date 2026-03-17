## CONTEXT HANDOFF - 2026-03-16 (Session 2)

### Session Summary
Security hardening + full bot identity setup. Moved API key to .env file. Rotated both tokens. Wrote IDENTITY.md, SOUL.md, USER.md, TOOLS.md, MEMORY.md — bot now knows its identity, all project paths, HA entity IDs, and cross-project status. Confirmed read tool works with absolute paths. Enabled exec + bash so bot can run gh, git, curl. Wired up HA REST API access. Set up daily-digest cron and nightly MEMORY.md refresh cron. Created chuggies510/openclaw GitHub repo, closed chuggies#53.

**Chat**: S2-openclaw-ha-tools-bash

### Changes made

| Change | Status |
|--------|--------|
| API key moved to ~/.openclaw/.env (chmod 600) | Done |
| Service updated to EnvironmentFile (key out of world-readable file) | Done |
| Bot token rotated via BotFather | Done |
| Gateway token rotated | Done |
| IDENTITY.md written | Done |
| SOUL.md written (includes full command list, behavior rules) | Done |
| USER.md written | Done |
| TOOLS.md written (project map, HA entities + automations, infra) | Done |
| MEMORY.md written (synthesized cross-project status) | Done |
| exec + bash enabled in tool deny list | Done |
| HA REST API access configured in TOOLS.md | Done |
| Daily digest cron — 8am Pacific, delivers to Telegram | Done |
| refresh-memory cron — 2am Pacific, rewrites MEMORY.md from memory banks | Done |
| GitHub repo chuggies510/openclaw created | Done |
| chuggies#53 closed | Done |
| CLAUDE.md updated: repo link, safety rules, Node version, IP | Done |
| Memory bank IPs corrected (192.168.3.11 → 192.168.3.4) | Done |

### Files modified
- `.claude/memory-bank/active-context.md` — session 2 handoff
- `CLAUDE.md` — repo link, updated safety rules
- `~/.openclaw/workspace/` — all workspace files (not in repo yet)
- `~/.openclaw/openclaw.json` — tokens, tool deny list
- `/etc/systemd/system/openclaw-gateway.service` — EnvironmentFile

### Test Status
No test suite.

### Knowledge extracted
- tech-context.md: Dev Pi SSH alias, Node 24, tool deny list, token locations, GitHub repo
- system-patterns.md: read tool not sandboxed, bootstrap file list, MEMORY.md pattern, cron setup
- CLAUDE.md: /new vs /compact gotcha, token rotation done

### Decisions recorded
None.

### Next Session Priority
Back up workspace files (SOUL.md, IDENTITY.md, TOOLS.md, MEMORY.md) to the GitHub repo. Then evaluate: OpenClaw vs handrolled Bun bot — write up findings after a week of use.

### Open Issues
- Workspace files not in repo — identity/config only on dev-pi filesystem (no backup)
- refresh-memory cron uses agent message prompt instead of direct script exec — may be unreliable

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
1. Rotate bot token (BotFather `/revoke`) + gateway token (`openclaw configure`)
2. Set up proactive cron — daily digest to Telegram
3. Evaluate: is OpenClaw worth keeping or build handrolled Bun bot?
