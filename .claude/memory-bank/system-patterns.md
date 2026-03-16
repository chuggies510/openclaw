# OpenClaw System Patterns

## Target Architecture

```
┌──────────────┐     ┌──────────────┐     ┌─────────────────┐
│  Telegram     │────▶│  OpenClaw     │────▶│  Claude API     │
│  (phone)      │◀────│  (Dev Pi)     │     │  (phase 1)      │
└──────────────┘     │              │     └─────────────────┘
                     │  - gateway    │     ┌─────────────────┐
                     │  - skills     │────▶│  Ollama (M4)    │
                     │  - memory     │     │  (phase 2)      │
                     └──────────────┘     └─────────────────┘
                            │
                     ┌──────┴──────┐
                     │ Chuggies    │
                     │ ecosystem   │
                     │ - memory    │
                     │   banks     │
                     │ - gh CLI    │
                     │ - git repos │
                     └─────────────┘
```

## Security Model

| Layer | Control |
|-------|---------|
| Network | Telegram polling (outbound only), no inbound ports |
| Auth | `dmPolicy: pairing` — only paired Telegram users can interact |
| Permissions | exec + bash enabled; write/edit/process/apply_patch denied |
| Cost | Anthropic console spend cap |
| Secrets | ~/.openclaw/.env (chmod 600), never in repo |

## Context Injection (session start)

OpenClaw injects a hardcoded list of workspace files into every agent session:
`AGENTS.md, SOUL.md, TOOLS.md, IDENTITY.md, USER.md, HEARTBEAT.md, BOOTSTRAP.md`

- **MEMORY.md** is read by AGENTS.md instruction at session start (not auto-injected)
- Custom filenames (e.g. PROJECTS.md) are ignored — must fold into the hardcoded list
- Read tool works with absolute paths outside workspace — not sandboxed

## Cron Jobs

| Job | Schedule | Purpose |
|-----|----------|---------|
| daily-digest (ac2c3d83) | 8am Pacific | Cross-project status → Telegram DM |
| refresh-memory (8d5cdeb7) | 2am Pacific | Rewrites MEMORY.md from all memory banks |

## Integration Points

| Asset | How the bot reads it |
|-------|---------------------|
| Memory banks | read tool, absolute path `~/2_project-files/projects/active-projects/{proj}/.claude/memory-bank/active-context.md` |
| Open issues | `gh issue list --repo chuggies510/{repo}` via exec/bash |
| HA entities/automations | `curl` against HA REST API at 192.168.3.3:8123 |
| HA token | `grep HA_API_TOKEN ~/2_project-files/_shared/secrets/chungus-net.env | cut -d= -f2` |

## Evaluation Criteria

| Question | Pass | Fail |
|----------|------|------|
| Can I install on ARM64 Dev Pi? | Runs natively or with minor tweaks | Requires x86, Docker-only, or heavy deps |
| Custom skill difficulty? | SKILL.md is intuitive, can shell out to our scripts | Need to learn complex TS abstractions |
| Telegram latency? | < 5s response time | Sluggish or unreliable |
| Memory system useful? | Adds value over our memory banks | Redundant or conflicting |
| Resource usage on Pi? | < 300MB RAM, low CPU | Heavy, competes with other services |
