# CLAUDE.md

Cross-project AI assistant accessible via Telegram. Evaluating OpenClaw as the platform — spike to understand its architecture, build a custom skill, and decide build-vs-adopt for a persistent, proactive development partner.

## Phase Plan

| Phase | Goal |
|-------|------|
| 1. Install & connect | OpenClaw on Dev Pi, Telegram round-trip, Claude API |
| 2. Custom skill | Cross-project status: reads memory banks, pulls issues across repos |
| 3. Proactive checks | Cron-driven daily digest via Telegram |
| 4. Evaluate | What was easy, what fought us, what's bloat |
| 5. Decision | Build own Bun service or continue with OpenClaw |

## Safety Rules

- API key spend capped in Anthropic console.
- `dmPolicy: pairing` auth gate — only paired Telegram users can interact.
- Telegram polling (not webhooks) — no inbound port exposure on Dev Pi.
- No secrets in repo. Use environment variables or `.env` (gitignored).
- Tool deny list: write, edit, process, apply_patch. exec + bash enabled.

## Tooling

- Use `bun` not npm/yarn/pnpm (for any custom code we write)
- OpenClaw itself uses pnpm — follow their conventions within their codebase

## Gotchas

- **OpenClaw caches API key** in `~/.openclaw/agents/main/agent/auth-profiles.json` — changing env var alone doesn't fix billing errors
- **Model config path** is `agents.defaults.model.primary` not `agents.main.model` — wrong path crashes gateway in restart loop
- **systemd-logind is masked** on DietPi physical hardware — user-level systemd won't work. Use system-level service instead.
- **`--install-daemon` silently skips** when user systemd unavailable — create `/etc/systemd/system/openclaw-gateway.service` manually
- **cmake required** before `npm install -g openclaw@latest` on Pi — needed for sqlite-vec native build

## GitHub

- **Repo**: https://github.com/chuggies510/openclaw
