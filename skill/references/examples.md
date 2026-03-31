# Real-World Examples

Examples of DITA-FEATURES.md from production projects.

## AIR Project (Context Compression)

AIR uses DITA to track 50+ features across multiple epics:

### Feature Organization

```yaml
features:
  # Core Compressors (F001-F012)
  - id: F001
    title: Web Content Compression
    summary: Extract and compress web page content, removing navigation and ads
    impl: [core/src/web/compressor.ts#compress, core/src/web/readability.ts]
    tests: [core/tests/web.test.ts#F001]
    depends: []
    designed_at: 2026-03-12T08:00:00Z
    implemented_at: 2026-03-12T14:00:00Z
    tested_at: 2026-03-12T16:00:00Z

  # CLI (F100-F199)
  - id: F101
    title: Direct Call Mode
    summary: Call compressors directly without subcommands
    impl: [cli/src/commands/direct.ts#run]
    tests: [cli/tests/direct.test.ts#F101]
    depends: [F001, F002, F003]
    designed_at: 2026-03-25T08:00:00Z
    implemented_at: 2026-03-25T10:00:00Z
    tested_at: 2026-03-25T12:00:00Z
```

### PRD Module Section

```markdown
### 3.1 Core Compressors

The compression engine that powers all AIR functionality. Each compressor
is specialized for a content type.

→ **Features**: F001-F012

**Compressor Types:**

| Type | Input | Compression | Key Technique |
|------|-------|-------------|---------------|
| Web | HTML/URL | 60-80% | Readability + structured extraction |
| Read | Source code | 40-60% | Line numbers + skeleton mode |
| Grep | Search results | 30-50% | Context grouping |

**Decision History:**

| Date | Decision | Reason |
|------|----------|--------|
| 2026-03-12 | Use Readability | Mozilla-maintained, proven algorithm |
| 2026-03-14 | Add skeleton mode | 90%+ reduction for large files |
```

## memory-in-box Project

memory-in-box tracks features across CLI, NPM package, and integrations:

### Feature ID Ranges

```yaml
# CLI Commands (F001-F099)
# NPM Package (F100-F199)
# Integrations (F200-F299)
# Future (F300+)
```

### Deprecation Example

```yaml
- id: F050
  title: SQLite Storage
  summary: Store memories in SQLite database
  impl: [src/sqlite-store.ts]
  tests: [tests/sqlite.test.ts#F050]
  depends: []
  designed_at: 2026-03-10T10:00:00Z
  implemented_at: 2026-03-11T14:00:00Z
  tested_at: 2026-03-11T16:00:00Z
  deprecated: true
  replaced_by: F001  # File-based storage
```

### Change History

```markdown
## 6. Change History

### 2026-03-17

- **F050 Deprecated**: SQLite storage replaced by file-based storage
  - Decision: Remove external dependencies
  - Impact: F050 deprecated, F001-F003 become primary
  - Reason: Zero-dependency goal, git-friendly storage

### 2026-03-15

- **F100-F120**: NPM package extracted from CLI
  - Decision: Publish as separate npm package
  - Impact: New feature range F100+
  - Reason: Enable programmatic usage
```

## agentcraft Project (HLC Framework)

agentcraft uses DITA to track the Human-Like Code framework development:

### Multi-Language Support

```yaml
- id: F101
  title: Python Detection
  summary: Detect Python language from file content
  impl: [src/detectors/python.py#detect]
  tests: [tests/test_python.py#F101]
  depends: [F001]  # Core Types
  designed_at: 2026-03-20T10:00:00Z
  implemented_at: 2026-03-21T14:00:00Z
  tested_at: null

- id: F102
  title: TypeScript Detection
  summary: Detect TypeScript language from file content
  impl: [src/detectors/typescript.py#detect]
  tests: [tests/test_typescript.py#F102]
  depends: [F001]
  designed_at: 2026-03-20T10:00:00Z
  implemented_at: null
  tested_at: null
```

### Dependency Graph

```
F001 (Core Types)
  ├── F101 (Python Detection)
  ├── F102 (TypeScript Detection)
  └── F103 (JavaScript Detection)
        ↓
F201 (Python Lexer)
F202 (TypeScript Lexer)
        ↓
F301 (Python Parser)
F302 (TypeScript Parser)
        ↓
F401 (HLC Converter)
```

## Tips from Real Usage

### 1. Feature Granularity

**Too coarse:**
```yaml
- id: F001
  title: User Management
  summary: All user-related functionality
```

**Just right:**
```yaml
- id: F001
  title: User Registration
  summary: Create new user accounts with email verification

- id: F002
  title: User Authentication
  summary: Login with email/password, returns JWT

- id: F003
  title: Password Reset
  summary: Email-based password reset flow
```

### 2. Dependency Declaration

Only declare **direct** dependencies:

```yaml
# Good - F003 directly uses F001
- id: F003
  depends: [F001]

# Bad - F003 transitively depends on F000 through F001
- id: F003
  depends: [F000, F001]  # Don't include F000
```

### 3. Test Organization

Match test files to feature groups:

```
tests/
├── auth.test.ts      # F001-F010 (Authentication)
├── user.test.ts      # F011-F020 (User Management)
└── api.test.ts       # F100-F150 (API Endpoints)
```

Each test file uses `describe('F###: ...'):

```typescript
// auth.test.ts
describe('F001: User Registration', () => { ... });
describe('F002: User Authentication', () => { ... });
describe('F003: Password Reset', () => { ... });
```
