# Project Instructions

## System Overview

Multi-agent delivery framework using GitHub Copilot agents. Eight specialist agents collaborate in a stateful, phased flow to deliver features from idea to production-ready code.

## Layers

| Layer | Location | Purpose |
|-------|----------|---------|
| Agents | `.github/agents/` | Role-specific AI agents |
| Skills | `.github/skills/` | Reusable instruction sets agents @-reference |
| Rules | `.github/rules/` | Coding & orchestration conventions |
| Context | `.github/context/` | Shared domain knowledge & vocabulary |
| Decisions | `.github/decisions/` | Architectural Decision Records |

## The Flow (Stateful, Linear, Phase-Based)

```
┌─────────────────────────────────────────────────────────┐
│  PHASE 1: UNDERSTAND                                    │
│                                                         │
│  Planner ─── grill-with-docs, domain-model,            │
│              ubiquitous-language, adr                   │
│                                                         │
│  Output: grilling-output.md, updated CONTEXT.md,       │
│          updated UBIQUITOUS_LANGUAGE.md, ADRs           │
├─────────────────────────────────────────────────────────┤
│  PHASE 2: SPECIFY                                      │
│                                                         │
│  Spec Writer ─── to-prd, domain-model                  │
│                                                         │
│  Output: prd.md (formal Product Requirements Document) │
├─────────────────────────────────────────────────────────┤
│  PHASE 3: DECOMPOSE                                    │
│                                                         │
│  Task Decomposer ─── vertical slicing, blocking deps   │
│                                                         │
│  Output: issues.md (ordered vertical-slice tasks)      │
├─────────────────────────────────────────────────────────┤
│  PHASE 4: BUILD (loop per issue)                       │
│                                                         │
│  Orchestrator dispatches per issue:                     │
│    → BE Developer (tdd, code-review)                   │
│    → FE Developer (tdd, code-review)                   │
│                                                         │
│  Output: implementation + passing tests per issue      │
├─────────────────────────────────────────────────────────┤
│  PHASE 5: REVIEW                                       │
│                                                         │
│  Architecture Reviewer ─── improve-architecture,       │
│                            code-review                  │
│                                                         │
│  Output: review findings, refactoring suggestions      │
└─────────────────────────────────────────────────────────┘
```

## Agent Roster

| # | Agent | Phase | Skills Used |
|---|-------|-------|-------------|
| 1 | Planner | Understand | grill-with-docs, ubiquitous-language, adr, context-update |
| 2 | Spec Writer | Specify | to-prd, domain-model |
| 3 | Task Decomposer | Decompose | (vertical slicing methodology) |
| 4 | Orchestrator | Build | context-update |
| 5 | BE Developer | Build | tdd, code-review |
| 6 | FE Developer | Build | tdd, code-review |
| 7 | Architecture Reviewer | Review | improve-architecture, code-review |
| 8 | Requirements | On-demand | template-extract |

## State Tracking (Two-Tier)

The Orchestrator maintains `state.yaml` with:
- **Top level:** Current phase (understand → specify → decompose → build → review)
- **Build phase:** Nested issue tracking (which issues are pending/in_progress/done)

## Key Principles

1. **Always grill first** — no spec without structured questioning.
2. **PRD before issues** — no tasks without a formal spec.
3. **Vertical slices** — each issue cuts through all layers, not horizontal slabs.
4. **TDD in development** — test first, implement, refactor.
5. **Architecture review after build** — catch structural decay before it accumulates.
6. **Skills are @-referenced** — agents invoke skills from `.github/skills/*.md`.
7. **Context evolves** — CONTEXT.md and UBIQUITOUS_LANGUAGE.md are living documents.
8. **Decisions are recorded** — non-obvious choices get an ADR.

## Stack

- **Backend:** FastAPI + SQLite
- **Frontend:** React + TypeScript + Vite
- **Testing:** pytest (BE), Vitest (FE)

## Quick References

- Shared vocabulary: `.github/context/UBIQUITOUS_LANGUAGE.md`
- Bounded context: `.github/context/CONTEXT.md`
- Orchestration rules: `.github/rules/rules-agent-decisions.md`
- Architecture: `.github/rules/rules-architecture.md`
