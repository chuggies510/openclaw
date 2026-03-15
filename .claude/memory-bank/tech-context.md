# OpenClaw Tech Context

## OpenClaw Overview

| Attribute | Value |
|-----------|-------|
| Repo | github.com/openclaw/openclaw |
| Language | TypeScript (Node 22, pnpm monorepo) |
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
| Dev Pi | Bot host (always on) | ARM64 Debian, 192.168.3.11 |
| Mac Mini M4 | LLM inference | 16GB, running Ollama + Qwen3:7B |
| 3900x | Workstation | Not always on |

## LLM Provider Plan

| Phase | Provider | Cost |
|-------|----------|------|
| 1 | Claude API (Sonnet) | ~$3/$15 per 1M tokens |
| 2 | Ollama on Mac Mini (Qwen3 or larger) | $0 |
