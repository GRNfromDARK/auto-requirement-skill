# Auto-Requirement Skill

Multi-agent requirement design skill for [Claude Code](https://claude.com/claude-code). Transform product ideas into complete, hierarchical, measurable requirement documents.

## Install

```bash
npx skills add GRNfromDARK/auto-requirement-skill
```

Or install globally:

```bash
npx skills add GRNfromDARK/auto-requirement-skill -g
```

## Usage

Trigger in Claude Code with any of these keywords:

```
需求设计：用户登录系统
autorequirement: payment system
autoneed: data sync pipeline
```

## Pipeline Positioning

```
auto-requirement → auto-todo → auto-dev
(产品决策层)        (工程任务层)   (代码实现层)
```

This skill produces requirement documents for **product decision-making** (Why, What, Priorities). Engineering decomposition and code implementation are handled by downstream skills.

## Key Features

- **Top-down hierarchical decomposition** — Goal tree: Strategic Goals → Capability Domains → Features → Acceptance Criteria
- **4+N parallel perspective agents** — Product, Technical, UX, Adversary + dynamic domain experts
- **Adaptive depth (3-5 levels)** — Light/Standard/Deep per branch based on complexity
- **Domain-depth artifacts** — Decision trees, computation specs, state machines, impact matrices (generated only where needed)
- **Lightweight cross-references** — `traces_to` / `depends_on` fields with auto-validated dependency summary
- **Structural validation** — Hierarchy completeness, reference consistency, and depth adequacy checks before final output
- **Technical architecture decisions** — Capture macro-level architecture choices (database, service architecture, frontend/backend separation) as part of requirements

## How It Works

```
Receive outline → Context analysis → Skills discovery → Contradiction scan
    → Hierarchical decomposition → 4 parallel agents → Synthesis & depth assessment
    → Questions (top-down order) → Iterate → Structural validation → Output document
```

## Output

Structured requirement document saved to `docs/requirements/YYYY-MM-DD-<topic>-requirement.md` with:

1. Executive Summary with measurable KPIs
2. Strategic Goals & Traceability (goal tree)
3. User Personas & Scenarios
4. Capability Domains & Feature Hierarchy (not flat lists!)
5. Non-Functional Requirements & Technical Architecture Decisions
6. Scope & Boundaries with rationale
7. Dependency Summary with critical path
8. Risks & Mitigations
9. Open Questions
10. Appendix (Decision Log, Domain-Depth Artifacts)

## Changelog

### v2.1 (2026-03-03)

- **Technical architecture decisions**: Auto-requirement now captures macro-level architecture choices (AR-xxx) — database selection, service architecture, frontend/backend separation — as technical requirements that constrain downstream engineering
- **Question limit increased**: Per-round question limit raised from 5 to 10, reducing unresolved items in the final document
- **Technical Agent enhanced**: Now identifies architecture decisions alongside feasibility analysis
- **Synthesis Agent updated**: Outputs architecture decision table from Technical Agent findings
- **Requirement template**: Added Section 5.6 "Technical Architecture Decisions" with Decision/Rationale/Alternatives/Downstream Impact columns
- **Structural validation**: Added architecture decision completeness check

### v2.0 (2026-03-03)

- Initial public release with hierarchical decomposition, adaptive depth, 4+N parallel agents, domain-depth artifacts, cross-references, and structural validation

## Documentation

See [skills/auto-requirement/README.md](skills/auto-requirement/README.md) for detailed documentation.

## License

MIT
