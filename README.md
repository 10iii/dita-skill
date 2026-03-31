# DITA — Design-Implementation-Test Alignment

A methodology for maintaining strict alignment between design, implementation, and testing in AI agent-driven development.

## What is DITA?

DITA helps AI agents (and humans) track features through their complete lifecycle:

```
Design → Implementation → Testing → (iterate)
```

Each project maintains a single `DITA-FEATURES.md` file that serves as:
- **Structured feature tracking** (YAML front matter)
- **Living PRD document** (Markdown body)

## Quick Example

```markdown
---
features:
  - id: F001
    title: User Login
    summary: Authenticate users with email/password
    impl: [auth.ts#login]
    tests: [auth.test.ts#F001]
    depends: []
    designed_at: 2026-03-20T10:00:00Z
    implemented_at: 2026-03-20T14:00:00Z
    tested_at: 2026-03-20T16:00:00Z
---

# Product Requirements Document

## 1. Product Vision

This project provides secure user authentication...

## 3. Modules

### 3.1 Authentication Module

→ **Features**: F001-F005

| Decision | Reason |
|----------|--------|
| Use JWT tokens | Stateless, scalable |
```

## Installation

### As an Agent Skill

**Claude Code / Claude.ai:**
1. Download this repo
2. Copy `skill/` folder to your skills directory
3. Or upload `skill/` as a zip to Claude.ai > Settings > Skills

**OpenCode / OpenClaw:**
```bash
cp -r skill ~/.opencode/skills/dita
# or
cp -r skill ~/.agents/skills/dita
```

### Manual Usage

Just create a `DITA-FEATURES.md` in your project root using the template in `skill/references/template.md`.

## Key Concepts

### Feature Status

Status is determined by the **most recent timestamp**:

| Status | Condition |
|--------|-----------|
| `draft` | All timestamps null |
| `designed` | `designed_at` is most recent |
| `implemented` | `implemented_at` is most recent |
| `tested` | `tested_at` is most recent |
| `deprecated` | `deprecated: true` |

### Feature Schema

| Field | Required | Description |
|-------|----------|-------------|
| `id` | ✅ | Unique ID (`F001`, `F002`, ...) |
| `title` | ✅ | Short title |
| `summary` | ✅ | 1-2 sentence description |
| `impl` | ✅ | Implementation: `[file#function]` |
| `tests` | ✅ | Tests: `[file#F###]` |
| `depends` | ✅ | Dependencies: `[F001, F002]` |
| `designed_at` | | ISO timestamp |
| `implemented_at` | | ISO timestamp |
| `tested_at` | | ISO timestamp |
| `deprecated` | | Boolean |

### PRD Integration

The Markdown body is a **living PRD** with:

1. Product Vision
2. Architecture Overview
3. Modules (each linked to Feature IDs)
4. Decision History tables
5. Change History

## Why DITA?

**For AI Agents:**
- Structured data for automated checks
- Clear implementation/test locations to grep
- Dependency graph for impact analysis

**For Humans:**
- Single source of truth (no separate PRD files)
- Decision traceability
- Clear feature status at a glance

## Projects Using DITA

- [AIR](https://github.com/10iii/air) — Context compression for LLMs
- [memory-in-box](https://github.com/10iii/memory-in-box) — Three-layer memory for AI agents
- [agentcraft](https://github.com/10iii/agentcraft) — Human-Like Code framework

## License

MIT

## Links

- [Anthropic Agent Skills Standard](https://agentskills.io)
- [Author's GitHub](https://github.com/10iii)
