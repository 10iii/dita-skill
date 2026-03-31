---
name: dita
description: >
  Design-Implementation-Test Alignment methodology for AI agent-driven development.
  Tracks features through design→implementation→testing phases with timestamps.
  Use when starting a new project that needs feature tracking, checking code changes
  for design-implementation-test consistency, querying feature status, or analyzing
  feature dependencies and change impact. Triggers: "DITA", "feature tracking",
  "design-implementation alignment", "功能追踪", "设计-实现-测试对齐".
trigger_keywords:
  - DITA
  - feature tracking
  - design-implementation alignment
  - 功能追踪
  - 设计-实现-测试对齐
  - design alignment
  - implementation status
  - test alignment
  - feature dependencies
  - change impact
  - 变更影响
  - 功能状态
  - PRD tracking
  - 设计文档
license: MIT
metadata:
  author: 10iii
  version: 1.2.0
  category: development-methodology
  tags: [feature-tracking, ai-agent, prd, tdd]
---

# DITA (Design-Implementation-Test Alignment)

A methodology for maintaining strict alignment between design, implementation, and testing in AI agent-driven development.

## Core Concept

Each project maintains a single `DITA-FEATURES.md` file that serves two purposes:

1. **YAML Front Matter** — Structured feature tracking (machine-readable)
2. **Markdown Body** — Living PRD document (human-readable)

## Quick Start

### 1. Create DITA-FEATURES.md

Use the template in `references/template.md` or create manually:

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
    implemented_at: null
    tested_at: null
---

# Product Requirements Document

## 1. Product Vision
...
```

### 2. Track Feature Lifecycle

```
Draft → Designed → Implemented → Tested
         ↑______________|  (can iterate back)
```

Update timestamps as you progress:
- `designed_at`: When design is finalized
- `implemented_at`: When code is complete
- `tested_at`: When tests pass

### 3. Link Modules to Features

In the Markdown PRD section, each module must reference its Feature IDs:

```markdown
### 3.1 Authentication Module

Handles user authentication and session management.

→ **Features**: F001-F005

**Decision History:**

| Date | Decision | Reason |
|------|----------|--------|
| 2026-03-20 | Use JWT tokens | Stateless, scalable |
```

## Feature Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | string | ✅ | Unique ID, format `F###` |
| `title` | string | ✅ | Short title |
| `summary` | string | ✅ | 1-2 sentence description |
| `impl` | string[] | ✅ | Implementation locations: `file#function` |
| `tests` | string[] | ✅ | Test locations: `file#F###` |
| `depends` | string[] | ✅ | Feature IDs this depends on |
| `designed_at` | datetime | | Design completion timestamp |
| `implemented_at` | datetime | | Implementation completion timestamp |
| `tested_at` | datetime | | Test completion timestamp |
| `deprecated` | boolean | | Whether feature is deprecated |
| `replaced_by` | string | | Feature ID that replaces this |

## Status Determination

Status is determined by the **most recent timestamp**:

```javascript
function getStatus(feature) {
  if (feature.deprecated) return 'deprecated';
  const times = [
    { name: 'tested', at: feature.tested_at },
    { name: 'implemented', at: feature.implemented_at },
    { name: 'designed', at: feature.designed_at },
  ].filter(t => t.at);
  if (times.length === 0) return 'draft';
  times.sort((a, b) => new Date(b.at) - new Date(a.at));
  return times[0].name;
}
```

## PRD Structure

The Markdown body should follow this structure:

1. **Product Vision** — Problem, users, value proposition
2. **Architecture Overview** — System diagram, tech stack
3. **Modules** — Each with Feature mapping and Decision History
4. **Non-Functional Requirements** — Performance, security
5. **Planned Features** — Roadmap
6. **Change History** — Chronological log of major changes

See `references/template.md` for the complete template.

## Agent Workflow

### After Code Changes

1. Parse DITA-FEATURES.md YAML
2. Check implementation exists (grep for functions in `impl`)
3. Check test coverage (grep for `describe('F###:` in tests)
4. Check dependency order (F002 can't be implemented before F001 if depends)
5. Report inconsistencies

### When to Update

- New feature → Add feature entry, set `designed_at`
- Code complete → Set `implemented_at`, update `impl`
- Tests pass → Set `tested_at`, update `tests`
- Re-design needed → Update `designed_at` (preserves history)
- Deprecate → Set `deprecated: true`

## Testing Convention

Use Feature ID in test descriptions:

```javascript
describe('F001: User Login', () => {
  it('should authenticate with valid credentials', () => { ... });
  it('should reject invalid password', () => { ... });
});
```

## Best Practices

1. **Single source of truth** — Don't maintain separate PRD files
2. **Decision traceability** — Every major decision has Date + Reason
3. **Feature linking** — All module sections reference Feature IDs
4. **ISO timestamps** — Use `YYYY-MM-DDTHH:mm:ssZ` format
5. **No line numbers** — Use `file#function`, not `file:42`

## References

- `references/template.md` — Complete DITA-FEATURES.md template
- `references/examples.md` — Real-world examples from AIR, memory-in-box projects
