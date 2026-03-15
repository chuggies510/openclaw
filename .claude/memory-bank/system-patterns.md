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
| Auth | ALLOWED_CHAT_IDS — reject before LLM |
| Permissions | Phase 1: read-only (memory banks, issues, git logs) |
| Cost | Anthropic console spend cap |
| Secrets | .env file, gitignored, never in repo |

## Integration Points

| Chuggies asset | How the bot reads it |
|----------------|---------------------|
| Memory banks | `cat ~/2_project-files/projects/active-projects/*/. claude/memory-bank/active-context.md` |
| Open issues | `gh issue list --repo chuggies510/{repo}` |
| Git activity | `git -C {repo_path} log --oneline -5` |
| Session history | Claude JSONL files (if accessible) |

## Evaluation Criteria

| Question | Pass | Fail |
|----------|------|------|
| Can I install on ARM64 Dev Pi? | Runs natively or with minor tweaks | Requires x86, Docker-only, or heavy deps |
| Custom skill difficulty? | SKILL.md is intuitive, can shell out to our scripts | Need to learn complex TS abstractions |
| Telegram latency? | < 5s response time | Sluggish or unreliable |
| Memory system useful? | Adds value over our memory banks | Redundant or conflicting |
| Resource usage on Pi? | < 300MB RAM, low CPU | Heavy, competes with other services |
