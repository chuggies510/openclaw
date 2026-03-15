# Memory Bank

Claude's context resets between sessions. The Memory Bank preserves understanding across that reset.

## Purpose

Each file serves a distinct role. Understanding the intent helps you route content correctly.

| File | Intent | Ask Yourself |
|------|--------|--------------|
| **project-brief.md** | WHY - Mission, constraints, success criteria | "What is this project trying to achieve?" |
| **active-context.md** | NOW - Current state, recent work, next steps | "What just happened? What should happen next?" |
| **tech-context.md** | WHAT - Facts to look up, reference data | "What will I need to look up later?" |
| **system-patterns.md** | HOW - Architecture, flows, connections | "How do these things work together?" |

**CLAUDE.md § Gotchas** (in project root) captures **WATCH OUT** - traps discovered through experience.

## File Guidance

### project-brief.md
The north star. Rarely changes. Contains mission, requirements, constraints, and success metrics.

### active-context.md
Changes every session. Contains current work focus and session handoffs.
The handoff is written for the NEXT Claude instance. What do they need to succeed?

### tech-context.md
Reference data - facts you look up, not narrative you read. Keep entries terse and scannable.

### system-patterns.md
Shows relationships and flows. How things connect, how data moves, how dependencies chain.

## Quality Principles

- **Bias toward inclusion**: Future Claude can skip irrelevant context but can't recover what wasn't captured
- **Terse over verbose**: Reference files should be scannable, not readable
- **For your replacement**: Write handoffs for the next Claude, not as documentation
