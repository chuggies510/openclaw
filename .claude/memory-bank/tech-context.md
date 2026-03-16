# OpenClaw Tech Context

## OpenClaw Overview

| Attribute | Value |
|-----------|-------|
| Repo | github.com/openclaw/openclaw |
| Language | TypeScript (Node 24, pnpm monorepo) |
| Stars | 145k+ |
| Former names | Clawdbot → MoltBot → OpenClaw |
| Creator | Peter Steinberger |

## Architecture (from research)

| Layer | Role |
|-------|------|
| Gateway | WebSocket server managing connections to messaging platforms |
| Agent | LLM reasoning engine |
| Skills | Modular capabilities (SKILL.md files — markdown + YAML frontmatter) |
| Memory | JSONL append-only sessions, embeddings, sqlite-vec vector search |

## Key Directories (openclaw repo)

| Path | Purpose |
|------|---------|
| `src/` | Core TS — gateway, memory, channels, context-engine, agents, sessions |
| `extensions/` | 44 channel plugins (telegram, discord, slack, etc.) |
| `skills/` | 53 skills (weather, gh-issues, spotify, obsidian, etc.) |
| `src/memory/` | 103 files — embeddings, vector search, sqlite-vec, MMR reranking |

## Skill Format

Skills are SKILL.md files with YAML frontmatter. The LLM reads the markdown at runtime and executes commands described within. No TypeScript in the skill itself.

## Infrastructure

| Machine | Role | Specs |
|---------|------|-------|
| Dev Pi | Bot host (always on) | ARM64 Debian, 192.168.3.4, SSH alias `dev-pi`, user `chris` |
| Infra Pi | HA + network services | 192.168.3.3, user `dietpi` |
| Mac Mini M4 | LLM inference | 16GB, running Ollama + Qwen3:7B |
| 3900x | Workstation | Not always on, 192.168.3.11 |

## Bot Config

| Setting | Value |
|---------|-------|
| Model | `anthropic/claude-haiku-4-5` |
| Config | `~/.openclaw/openclaw.json` |
| API key | `~/.openclaw/.env` (chmod 600), EnvironmentFile in service |
| Service | `/etc/systemd/system/openclaw-gateway.service` |
| Workspace | `~/.openclaw/workspace/` |
| Sessions | `~/.openclaw/agents/main/sessions/` |
| Tool deny list | write, edit, process, apply_patch |
| Tool allow | exec, bash, read (+ all others not denied) |

## GitHub

- **Chuggies Bot repo**: https://github.com/chuggies510/openclaw

## LLM Provider Plan

| Phase | Provider | Cost |
|-------|----------|------|
| 1 | Claude API (Haiku 4.5) | cheap |
| 2 | Ollama on Mac Mini (Qwen3 or larger) | $0 |
