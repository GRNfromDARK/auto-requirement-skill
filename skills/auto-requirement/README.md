# Auto-Requirement: Multi-Agent Requirement Design Skill

## What is this?

Auto-Requirement is a Claude Code skill that transforms rough product ideas or requirement outlines into complete, measurable, and verifiable requirement documents through **multi-agent parallel exploration**, **self-debate**, **hierarchical decomposition**, and **iterative user clarification**.

**Core principle:** Hierarchical decomposition → Multiple perspective agents think independently → debate and synthesize → present unified questions to user → iterate with adaptive depth → produce optimized requirement document.

### Pipeline Positioning

Auto-Requirement is the **first step** in a three-stage development pipeline:

```
auto-requirement → auto-todo → auto-dev
(产品决策层)        (工程任务层)   (代码实现层)
```

- **auto-requirement**: Product decision-making — Why, What, and Priorities
- **auto-todo**: Engineering decomposition — How to break down and assign tasks
- **auto-dev**: Code implementation — TDD pipeline with gated development

## Quick Start

Trigger with any of these keywords in Claude Code:

```
需求设计：用户登录系统
autorequirement: payment system
autoneed: data sync pipeline
```

Simply provide a brief product direction or requirement outline, and the skill will guide you through the entire requirement design process.

## Key Features (v2)

### 1. Top-Down Hierarchical Decomposition
Before dispatching agents, the skill builds a **goal tree** from your outline:
```
SG-1: [Strategic Goal]
├── CD-1.1: [Capability Domain] [complexity: Standard]
│   ├── FR-001: [Feature] (Must)
│   └── FR-002: [Feature] (Should)
└── CD-1.2: [Capability Domain] [complexity: Deep]
    └── FR-003: [Feature] (Must)
```
All agents anchor their analysis to this tree. Every finding, question, and recommendation maps to a specific node.

### 2. Adaptive Depth (3-5 Levels)
Not every domain needs the same depth. The skill classifies each branch:
- **Light** (1-3 features, well-understood) → 3-level depth
- **Standard** (4-8 features, some unknowns) → 4-level depth with acceptance criteria
- **Deep** (complex business logic, multiple states) → 5-level depth with decision trees, state machines, computation rules

### 3. Domain-Depth Artifacts
For branches classified as "Deep", the skill produces:

| Artifact | When Used |
|----------|-----------|
| **Decision Trees** | Branching classification logic (e.g., event type classification) |
| **Computation Specifications** | Non-trivial calculations with formal rules (e.g., PnL using WAC) |
| **State Machines** | Entities with 3+ lifecycle states (e.g., order lifecycle) |
| **Impact Matrices** | Actions affecting multiple data entities or downstream systems |

### 4. Lightweight Cross-References
Every requirement includes:
- `traces_to:` — which strategic goal it serves
- `depends_on:` — what other requirements it needs
- Auto-generated dependency summary with critical path

### 5. Structural Validation
Before producing the final document, a validation pass checks:
- Hierarchy completeness (every goal has domains, every domain has features)
- Cross-reference consistency (no broken links)
- Depth adequacy (Deep branches have their required artifacts)
- Priority coverage (Must items are fully specified)

## How It Works

### 12 Phase Process

```
 ┌───────────────────────────┐
 │  1. Receive outline        │
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐
 │  2. Context analysis       │  Read project files, git history, tech stack
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐
 │  3. Skills discovery       │  Search for domain-relevant skills
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐
 │  4. Contradiction scan     │  Detect logical conflicts in outline
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐
 │  4.5 Hierarchical          │  Build top-down goal tree with
 │      decomposition  [NEW]  │  adaptive depth assessment
 └───────────┬───────────────┘
             ▼
 ┌──────────────────────────────────────────────────────────────────┐
 │  5. Multi-agent parallel exploration (4 core + domain experts)  │
 │     Agents receive goal tree and organize output by nodes       │
 │     ┌────────┐ ┌────────┐ ┌──────┐ ┌───────────┐ ┌ ─ ─ ─ ─ ┐ │
 │     │Product │ │  Tech  │ │  UX  │ │ Adversary │  Domain*   │
 │     └───┬────┘ └───┬────┘ └──┬───┘ └─────┬─────┘ └ ─ ┬ ─ ─ ┘ │
 └─────────┼──────────┼─────────┼────────────┼──────────┼────────┘
           └──────────┼─────────┘            │          │
                      ▼                      │          │
 ┌────────────────────────────────────────┐  │          │
 │  6. Synthesis, debate &                │◄─┘──────────┘
 │     depth assessment  [ENHANCED]       │
 └───────────┬────────────────────────────┘
             ▼
 ┌───────────────────────────┐
 │  7. Present unified Q&A    │  Top-down order, max 10 per round
 │     + progress indicator   │
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐     ┌───────────────────────────────┐
 │  8. Completeness &         │─NO─▶│ Targeted depth for flagged    │
 │     depth check            │     │ branches → Back to 5          │
 └───────────┬───────────────┘     └───────────────────────────────┘
             │ YES
             ▼
 ┌───────────────────────────┐
 │  9. Structural validation  │  [NEW] Hierarchy + references check
 │                            │
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐
 │ 10. Produce requirement    │
 └───────────┬───────────────┘
             ▼
 ┌───────────────────────────┐     ┌────────────────┐
 │ 11. User review & approval │─NO─▶│ Back to Phase 5│
 └───────────┬───────────────┘     └────────────────┘
             │ YES
             ▼
          Done
```

### The 4+N Perspective Agents

**4 Core Agents (always present):**

| Agent | Perspective | Focus |
|-------|-------------|-------|
| **Product Agent** | Business value & decision framing | Goal validation, MoSCoW prioritization, success metrics, scope trade-offs |
| **Technical Agent** | Feasibility & depth detection | Architecture fit, complexity classification, **identifies where decision trees/state machines are needed** |
| **UX Agent** | User experience & journey mapping | User journeys mapped to features, journey-feature matrix, interaction patterns |
| **Adversary Agent** | Devil's advocate & depth audit | Challenge assumptions, **audit hierarchy quality**, challenge depth adequacy |

**Dynamic Domain Expert Agent (conditional):**

| Agent | Perspective | Focus |
|-------|-------------|-------|
| **Domain Expert Agent** | Domain-specific | Deep expertise + **produces domain-depth artifacts** (decision trees, computation rules, etc.) |

All agents run **in parallel**, receive the **goal tree**, and organize output **by tree nodes**. The **Synthesis Agent** then:
- Builds unified hierarchy from all agent outputs
- Resolves conflicts with evidence
- **Assesses depth per branch** (completeness score, artifact needs)
- Deduplicates and prioritizes questions in **top-down order**
- Detects new domains for expert agents

## Output Format

The final output is a structured, hierarchical requirement document:

```
docs/requirements/YYYY-MM-DD-<topic>-requirement.md
```

| # | Section | Content |
|---|---------|---------|
| 1 | Executive Summary | Problem, solution, measurable KPIs |
| 2 | **Strategic Goals & Traceability** | Top-level goals with goal→domain→feature tree |
| 3 | User Personas & Scenarios | Who, why, how — linked to goals |
| 4 | **Capability Domains & Feature Hierarchy** | Features organized by domain (not flat list!) |
| 5 | Non-Functional Requirements | Performance, security, scalability |
| 6 | Scope & Boundaries | In-scope, out-of-scope, non-goals with rationale |
| 7 | **Dependency Summary** | Auto-generated from cross-references + critical path |
| 8 | Risks & Mitigations | Per-domain and cross-cutting risks |
| 9 | Open Questions | Remaining TBDs |
| 10 | Appendix | Decision log, glossary, domain-depth artifacts |

### Quality Standards

- Every requirement is **concrete and measurable** (no "fast", "easy", "intuitive")
- Every Must-priority feature has **acceptance criteria** (Given/When/Then)
- Every assumption is **explicitly stated**
- Priority uses **MoSCoW** at both domain and feature level
- Every feature has `traces_to` and `depends_on` fields
- Domain-depth artifacts include **concrete examples**

## File Structure

```
auto-requirement/
├── README.md                              ← This file
├── SKILL.md                               ← Skill definition (loaded by Claude Code)
└── references/
    ├── perspective-prompts.md             ← Complete prompt templates for all agents
    └── requirement-template.md            ← Final requirement document template
```

## Hard Gate

**Enforced constraint:** The skill will **NOT** produce the final requirement document until ALL `[NEEDS CLARIFICATION]` markers are resolved AND structural validation passes AND the user confirms requirements are complete.

## Example

Here is a real requirement document produced by this skill:

- [Go Pipeline Requirements (Standalone)](../2026-03-03-go-pipeline-requirements-standalone-zh.md) — A complete requirement spec for an on-chain trade data processing pipeline, covering 22 functional requirements, 7 non-functional requirements, data contracts, DDL baseline, and Go interface inventory.

## Installation

Place the `auto-requirement/` folder in either location:

```bash
# Project-level (current project only)
.claude/skills/auto-requirement/

# User-level (all projects)
~/.claude/skills/auto-requirement/
```

Claude Code will automatically discover and load the skill.

## Tips

1. **The more detailed your outline, the better the output** — even a rough bullet list is far better than a one-liner
2. **Answer each round of questions carefully** — these questions survived 4-agent debate and are high-impact
3. **Questions follow top-down order** — strategic decisions first, implementation details last
4. **Depth is adaptive** — simple features stay lean, complex business logic gets decision trees and state machines
5. **The goal tree is your map** — it's built early and updated each round, so you always know where you are
6. **Cross-references are validated** — broken dependency links are caught before the final document

---

# Auto-Requirement：多 Agent 需求设计 Skill

## 这是什么？

Auto-Requirement 是一个 Claude Code skill，通过**多 Agent 并行探索、层级分解、自辩论与迭代澄清**，将粗略的产品想法或需求大纲转化为完整、可量化、可验收的需求文档。

**核心理念：** 层级分解 → 多 Agent 独立思考 → 综合辩论 → 向用户提出统一问题 → 自适应深度迭代 → 输出优化的需求文档。

### 流水线定位

Auto-Requirement 是三阶段开发流水线的**第一步**：

```
auto-requirement → auto-todo → auto-dev
(产品决策层)        (工程任务层)   (代码实现层)
```

- **auto-requirement**：产品决策 — 为什么做、做什么、优先级和取舍
- **auto-todo**：工程分解 — 怎么拆解和分配任务
- **auto-dev**：代码实现 — TDD 门控开发流水线

## 快速开始

在 Claude Code 中使用以下任一关键词触发：

```
需求设计：用户登录系统
autorequirement: 支付功能
autoneed: 数据同步管道
```

## 核心特性（v2）

### 1. 自顶向下层级分解
在派遣 Agent 之前，构建**目标树**：
```
SG-1: [战略目标]
├── CD-1.1: [能力域] [复杂度: 标准]
│   ├── FR-001: [特性] (Must)
│   └── FR-002: [特性] (Should)
└── CD-1.2: [能力域] [复杂度: 深度]
    └── FR-003: [特性] (Must)
```

### 2. 自适应深度（3-5 级）
- **轻量**: 3 级深度（简单功能）
- **标准**: 4 级深度 + 验收标准
- **深度**: 5 级深度 + 决策树、状态机、计算规范

### 3. 领域深度产物
深度分支自动生成：决策树、计算规范、状态机、影响矩阵

### 4. 轻量交叉引用
每条需求包含 `traces_to` 和 `depends_on` 字段，自动生成依赖摘要和关键路径

### 5. 结构验证
最终文档生成前验证：层级完整性、引用一致性、深度充分性、优先级覆盖

## 安装

将 `auto-requirement/` 文件夹放置到以下任一位置：

```bash
# 项目级
.claude/skills/auto-requirement/

# 用户级
~/.claude/skills/auto-requirement/
```

## 使用技巧

1. **提供的大纲越详细，质量越高**
2. **认真回答每轮问题** — 经过 4 Agent 辩论筛选的高影响力问题
3. **问题遵循自顶向下顺序** — 先战略决策，后实现细节
4. **深度自适应** — 简单功能保持精简，复杂业务逻辑获得决策树和状态机
5. **目标树是你的地图** — 早期构建，每轮更新，始终知道当前进展
6. **交叉引用经过验证** — 断裂的依赖链在最终文档前被捕获
