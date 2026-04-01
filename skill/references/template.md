---
# ============================================================
# YAML Front Matter: Structured Feature Data (Machine-First)
# ============================================================
# 
# 每个 Feature 是一个可独立测试的功能单元。
# 字段说明详见 SKILL.md 的 "Feature 字段说明" 章节。
#
features:
  # ============================================
  # Module 1: [模块名称]
  # ============================================
  
  - id: F001
    title: 功能标题
    summary: 简短功能描述（1-2 句）
    impl: [file.ts#FunctionName]
    tests: [file.test.ts#F001]
    depends: []
    designed_at: null
    implemented_at: null
    tested_at: null

  - id: F002
    title: 另一个功能
    summary: 依赖 F001 的功能
    impl: [file.ts#AnotherFunction]
    tests: [file.test.ts#F002]
    depends: [F001]
    designed_at: null
    implemented_at: null
    tested_at: null

  # ============================================
  # Module 2: [另一模块]
  # ============================================
  
  - id: F100
    title: 模块2的功能
    summary: 描述
    impl: []
    tests: []
    depends: []
    designed_at: null
    implemented_at: null
    tested_at: null

  # ============================================
  # Bug Fixes (B-series)
  # ============================================
  
  - id: B001
    title: Bug 修复标题
    summary: Bug 描述和修复方案
    impl: [file.ts#FixFunction]
    tests: [file.test.ts#B001]
    depends: []
    severity: P0  # P0(critical), P1(high), P2(medium), P3(low)
    designed_at: null
    implemented_at: null
    tested_at: null
---

# {项目名称} Product Requirements Document

> **DITA 方法论**：Design-Implementation-Test Alignment
> 
> 本文档是项目的**单一真相源**。关于设计/开发/测试/进度/计划/TODO/讨论/决议/变动的内容，
> 如不在本文档或其引用的文档中，均视为未生效/临时性/随时可删除。

---

## 1. Product Vision

{产品愿景和核心价值主张}

### 1.1 Problem Statement

{要解决的问题}

### 1.2 Target Users

{目标用户群体}

### 1.3 Core Value Proposition

{核心价值主张 - 为什么用户要用这个产品}

---

## 2. Architecture Overview

{架构图或 ASCII art}

```
project/
├── module1/        # [模块1说明]
│   ├── src/
│   └── tests/
├── module2/        # [模块2说明]
└── shared/         # [共享代码]
```

### 2.1 Tech Stack

- **Runtime**: {Node.js / Bun / Deno / ...}
- **Language**: {TypeScript / JavaScript / ...}
- **Testing**: {Vitest / Jest / ...}
- **Build**: {tsup / esbuild / ...}

---

## 3. Modules

### 3.1 {模块1名称}

{模块描述、设计目标、核心职责}

→ **Features**: F001-F00x

**Key Design Decisions:**

| Decision | Rationale |
|----------|-----------|
| 决策内容 | 决策原因 |

---

### 3.2 {模块2名称}

{模块描述}

→ **Features**: F100-F10x

---

### 3.x [ARCHIVED] {已归档模块} ⛔

{为什么归档的说明}

→ **Features**: Fxxx (deprecated)

**Decision History:**

| Date | Decision | Impact | Reason |
|------|----------|--------|--------|
| YYYY-MM-DD | 归档此模块 | Fxxx deprecated | 具体原因 |

---

## 4. Non-Functional Requirements

### 4.1 Performance

- {性能要求}

### 4.2 Security

- {安全要求}

### 4.3 Compatibility

- {兼容性要求}

---

## 5. Planned Features

{规划中但未实现的功能}

→ **Features**: Fxxx (designed_at = null or implemented_at = null)

| Feature | Priority | Notes |
|---------|----------|-------|
| Fxxx: 功能名 | P1 | 备注 |

---

## 6. Change History

> 按时间倒序记录重大变更。每条变更应关联受影响的 Feature ID。

### YYYY-MM-DD

- **{变更标题}**
  - Decision: {决策内容}
  - Impact: {影响的 Feature ID}
  - Reason: {决策原因}

### YYYY-MM-DD

- **{另一变更}**
  - ...

---

## 7. Status Legend

| 状态 | 判断规则 |
|------|----------|
| draft | 三个时间戳都为 null |
| designed | designed_at 是最新的时间戳 |
| implemented | implemented_at 是最新的时间戳 |
| tested | tested_at 是最新的时间戳 |
| deprecated | deprecated: true |

---

## 8. Feature ID Ranges

| Range | Module | Notes |
|-------|--------|-------|
| F001-F0xx | Module 1 | {说明} |
| F100-F1xx | Module 2 | {说明} |
| Fxxx+ | Planned | {说明} |
| B001+ | Bug Fixes | {说明} |

---

## 9. Notes

{当前状态摘要、临时备注、待办事项等}

---

## 10. Referenced Documents

> 外置的清单性/接口性/契约性文档。这些文档必须在此处引用才视为有效。

| Document | Type | Purpose | Status |
|----------|------|---------|--------|
| `docs/runtime-contract.json` | Contract | 运行时约定 | active |
| `docs/universal-interface.md` | Interface | 通用接口定义 | active |
| `docs/opcodes.csv` | Resource List | 协议操作码清单 | active |
| `openapi.yaml` | API Schema | REST API 定义 | draft |

**Document Types:**
- **Contract**: 组件间约定，机器可解析
- **Interface**: 接口定义，人机可读
- **Resource List**: 清单/枚举，数据性质
- **API Schema**: OpenAPI/GraphQL 等标准格式

---

(End of file)
