# Tiered Bot Awareness Implementation Plan

> **For agentic workers:** REQUIRED: Use superpowers-extended-cc:subagent-driven-development (if subagents available) or superpowers-extended-cc:executing-plans to implement this plan. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Make every repo in the Chuggies ecosystem aware of @chuggies_bot at the appropriate tier level.

**Architecture:** Two tiers of awareness. Tier 1 (low) adds a short CLAUDE.md section to all repos. Tier 2 (high) adds the same CLAUDE.md section plus a detailed tech-context.md memory bank section to chuggies, chungus-net, and ChuggiesMart. Bootstrap skill updated to inject Tier 1 for new projects.

**Tech Stack:** Markdown files (CLAUDE.md, tech-context.md), ChuggiesMart SKILL.md template

**Spec:** `docs/superpowers/specs/2026-03-17-tiered-bot-awareness-design.md`

---

## Chunk 1: Tier 1 — Low-Awareness Repos

Add the `## Chuggies Bot` section to CLAUDE.md in each low-awareness repo. Append to end of file. Four repos lack CLAUDE.md entirely (chungus-blog, pro-ject-pack, meap3, mo-marketing) — skip those, they'll get it when bootstrapped. test-boot is a test scaffold, not a real project — skip it. Client projects under `projects/client-projects/` are excluded (AC1 scopes to `active-projects/` only).

### Task 1: Add Tier 1 block to repos with existing CLAUDE.md

**Files (all appended to end):**
- Modify: `~/2_project-files/projects/active-projects/meap2-it/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/home-assistant/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/hire-chungus/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/podly-eval/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/meap-cli-cc/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/feature-dev-harnessed/CLAUDE.md`

**Block to append:**

```markdown

## Chuggies Bot

@chuggies_bot is a Telegram-based AI assistant that reads memory banks and issues across repos. It runs on Dev Pi (192.168.3.4) via OpenClaw. Your memory bank handoffs are consumed by the bot's nightly refresh (2am Pacific) — keep active-context.md structured and current.
```

- [ ] **Step 1: Append Tier 1 block to all 6 CLAUDE.md files**

For each file, read the current content first, then append the block above to the end. Verify each file doesn't already contain `## Chuggies Bot` before appending.

- [ ] **Step 2: Verify all 6 files have the block**

Run: `grep -l "## Chuggies Bot" ~/2_project-files/projects/active-projects/{meap2-it,home-assistant,hire-chungus,podly-eval,meap-cli-cc,feature-dev-harnessed}/CLAUDE.md`
Expected: All 6 paths listed.

- [ ] **Step 3: Commit in each repo**

For each of the 6 repos, `cd` into the repo and commit:
```bash
git add CLAUDE.md && git commit -m "docs: add Chuggies Bot awareness section to CLAUDE.md"
```

---

## Chunk 2: Tier 2 — High-Awareness Repos

Add the CLAUDE.md block (same as Tier 1) and the detailed tech-context.md section to chuggies, chungus-net, and ChuggiesMart. openclaw is already covered.

### Task 2: Add Tier 1 CLAUDE.md block to high-awareness repos

**Files (all appended to end):**
- Modify: `~/2_project-files/projects/active-projects/chuggies/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/chungus-net/CLAUDE.md`
- Modify: `~/2_project-files/projects/active-projects/ChuggiesMart/CLAUDE.md`

- [ ] **Step 1: Append Tier 1 block to all 3 CLAUDE.md files**

Same block as Task 1. Read first, verify no existing `## Chuggies Bot`, then append.

- [ ] **Step 2: Verify**

Run: `grep -l "## Chuggies Bot" ~/2_project-files/projects/active-projects/{chuggies,chungus-net,ChuggiesMart}/CLAUDE.md`
Expected: All 3 paths listed.

**Do not commit yet** — Task 3 adds tech-context.md changes and commits both files together.

### Task 3: Add Tier 2 tech-context.md section to high-awareness repos

**Files (all appended to end):**
- Modify: `~/2_project-files/projects/active-projects/chuggies/.claude/memory-bank/tech-context.md`
- Modify: `~/2_project-files/projects/active-projects/chungus-net/.claude/memory-bank/tech-context.md`
- Modify: `~/2_project-files/projects/active-projects/ChuggiesMart/.claude/memory-bank/tech-context.md`

**Section to append:**

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

- [ ] **Step 1: Append Tier 2 section to all 3 tech-context.md files**

Read each file first, verify no existing `## Chuggies Bot` section, then append.

- [ ] **Step 2: Verify**

Run: `grep -l "Chuggies Bot" ~/2_project-files/projects/active-projects/{chuggies,chungus-net,ChuggiesMart}/.claude/memory-bank/tech-context.md`
Expected: All 3 paths listed.

- [ ] **Step 3: Commit in each repo**

For each of the 3 repos (chuggies, chungus-net, ChuggiesMart), commit both files:
```bash
git add CLAUDE.md .claude/memory-bank/tech-context.md && git commit -m "docs: add Chuggies Bot awareness (Tier 2) to CLAUDE.md and tech-context.md"
```

---

## Chunk 3: Bootstrap Skill Update

Update ChuggiesMart's `/bootstrap-project` SKILL.md to inject the Tier 1 block into generated CLAUDE.md files.

### Task 4: Update bootstrap-project skill template

**Files:**
- Modify: `~/2_project-files/projects/active-projects/ChuggiesMart/workspace-toolkit/skills/bootstrap-project/SKILL.md`

- [ ] **Step 1: Read current SKILL.md**

Read the full file to locate the CLAUDE.md template in Step 5.

- [ ] **Step 2: Add Chuggies Bot section to the CLAUDE.md template**

In Step 5's CLAUDE.md template, add the following after the last conditional section (GitHub) and before the closing of the template:

```markdown
## Chuggies Bot

@chuggies_bot is a Telegram-based AI assistant that reads memory banks and issues across repos. It runs on Dev Pi (192.168.3.4) via OpenClaw. Your memory bank handoffs are consumed by the bot's nightly refresh (2am Pacific) — keep active-context.md structured and current.
```

This is unconditional — all bootstrapped projects are Chuggies ecosystem projects.

- [ ] **Step 3: Verify the section appears in the template**

Run: `grep "Chuggies Bot" ~/2_project-files/projects/active-projects/ChuggiesMart/workspace-toolkit/skills/bootstrap-project/SKILL.md`
Expected: At least one match.

- [ ] **Step 4: Bump ChuggiesMart version and commit**

Follow ChuggiesMart versioning conventions. Bump patch version, update CHANGELOG, commit, and push (plugin edits require push per CLAUDE.md rules).

---

## Chunk 4: Verification

### Task 5: Final acceptance check

- [ ] **Step 1: Verify all Tier 1 repos**

```bash
for repo in meap2-it home-assistant hire-chungus podly-eval meap-cli-cc feature-dev-harnessed chuggies chungus-net ChuggiesMart; do
  grep -q "## Chuggies Bot" ~/2_project-files/projects/active-projects/$repo/CLAUDE.md && echo "OK: $repo" || echo "MISSING: $repo"
done
```
Expected: All 9 show OK.

- [ ] **Step 2: Verify all Tier 2 repos**

```bash
for repo in chuggies chungus-net ChuggiesMart; do
  grep -q "Chuggies Bot (@chuggies_bot)" ~/2_project-files/projects/active-projects/$repo/.claude/memory-bank/tech-context.md && echo "OK: $repo" || echo "MISSING: $repo"
done
```
Expected: All 3 show OK.

- [ ] **Step 3: Verify bootstrap skill**

```bash
grep -q "Chuggies Bot" ~/2_project-files/projects/active-projects/ChuggiesMart/workspace-toolkit/skills/bootstrap-project/SKILL.md && echo "OK: bootstrap" || echo "MISSING: bootstrap"
```
Expected: OK.
