# OpenClaw Project Brief

## Mission

Evaluate OpenClaw (formerly MoltBot) as a platform for a cross-project AI assistant accessible via Telegram. The assistant should provide cross-project awareness across the Chuggies ecosystem, proactive notifications, and act as an always-on development partner.

## Core Requirements

| Category | Requirement |
|----------|-------------|
| Messaging | Telegram bot — text from phone, get intelligent responses |
| Cross-project awareness | Read memory banks, issues, git state across all repos |
| Proactive notifications | Git activity, stale issues, deploy health, daily digests |
| Schedule/email | Calendar and email triage via text |
| Security | Read-only phase 1, auth gate, no inbound ports, API spend cap |
| Cost path | Start Claude API, migrate to local Ollama on Mac Mini M4 |

## Constraints

- Runs on Dev Pi (192.168.3.4) — ARM64 Debian, always on
- Mac Mini M4 (16GB) available for LLM inference — currently running Qwen3:7B for podcast processing
- Must not expose inbound ports (Telegram polling, not webhooks)
- Phase 1 is read-only — no shell execution from LLM responses
- This is a spike/evaluation — not committed to OpenClaw long-term

## Success Metrics

- Text "what's happening across my repos?" from phone, get a useful answer
- Receive a daily digest of cross-project activity without asking
- Understand OpenClaw's architecture well enough to make an informed build-vs-adopt decision
- Total evaluation: 3-5 sessions

## Alternatives Considered

| Option | Verdict |
|--------|---------|
| OpenClaw (adopt) | Evaluating now — mature platform, but heavy (200+ TS files, 80 deps) |
| Handrolled Bun bot | Fallback — 100 lines, tight ecosystem integration, same learning |
| Hybrid (OpenClaw gateway + our scripts) | Possible middle ground — TBD after evaluation |
