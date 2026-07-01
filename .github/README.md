# Agentic Flow — Simplified Multi-Agent Delivery System

A stateful, phase-based multi-agent framework for GitHub Copilot. Eight specialist agents collaborate to take a feature from idea to production-ready code, with clear hand-offs, persistent state, and test-driven development.

## Philosophy

AI agents are capable engineers with one critical flaw: **no memory between sessions**. This system solves that by:

1. **Persisting everything to files** — every phase produces a markdown/YAML artifact that the next phase reads
2. **Shared vocabulary** — a glossary that code, agents, and humans all use consistently
3. **Structured questioning before coding** — grill-with-docs methodology prevents wasted effort
4. **TDD enforcement** — test-first development produces reliable, verifiable code
5. **Stateful progress tracking** — pick up exactly where you left off in any new chat

Inspired by [Matt Pocock's AI Hero skills](https://www.aihero.dev/5-agent-skills-i-use-every-day) and [grill-with-docs](https://www.aihero.dev/grill-with-docs).

---

## The Flow

### Overview

```
USER IDEA
    │
    ▼
┌─────────────────────────────────────────────────┐
│  PHASE 1: UNDERSTAND          (Chat 1)          │
│  Agent: Planner                                  │
│  Method: grill-with-docs                         │
│  Output: grilling-output.md                      │
├─────────────────────────────────────────────────┤
│  PHASE 2: SPECIFY             (Chat 1)          │
│  Agent: Spec Writer                              │
│  Method: to-prd                                  │
│  Output: prd.md                                  │
├─────────────────────────────────────────────────┤
│  PHASE 3: DECOMPOSE           (Chat 1)          │
│  Agent: Task Decomposer                          │
│  Method: vertical slicing                        │
│  Output: issues.md                               │
├─────────────────────────────────────────────────┤
│  PHASE 4: BUILD               (Chat 2, 3, ...)  │
│  Agent: Orchestrator → BE Dev → FE Dev           │
│  Method: TDD (red-green-refactor)                │
│  Output: implementation + passing tests          │
├─────────────────────────────────────────────────┤
│  PHASE 5: REVIEW              (Final Chat)      │
│  Agent: Architecture Reviewer                    │
│  Method: improve-architecture                    │
│  Output: review.md                               │
└─────────────────────────────────────────────────┘
    │
    ▼
PRODUCTION-READY CODE
```

### Why Multiple Chats?

The grilling session (Phase 1) consumes significant context window budget. By persisting all decisions to files, implementation can happen in **fresh chats** that read from those files without needing conversation history.

| Chat | Phases | Why |
|------|--------|-----|
| Chat 1 | Understand + Specify + Decompose | Grilling eats context; do all planning here |
| Chat 2+ | Build (one or more issues per chat) | Fresh context, reads issues.md + state.yaml |
| Final | Review | Fresh eyes on the completed codebase |

---

## Detailed Phase Breakdown

### Phase 1: UNDERSTAND (Planner)

The user describes what they want to build. The Planner **never assumes** — it grills:

1. Reads existing `CONTEXT.md` and `UBIQUITOUS_LANGUAGE.md`
2. Asks 1–3 questions at a time, walking down the design tree
3. Challenges fuzzy language ("what specifically do you mean by X?")
4. Surfaces terminology collisions with existing vocabulary
5. Discusses concrete scenarios (given/when/then)
6. Continues until all branches are resolved or user says "proceed"

**Produces:**
- `.github/working/grilling-output.md` — decisions, domain sketch, scenarios
- Updated `.github/context/UBIQUITOUS_LANGUAGE.md` — new/refined terms
- Updated `.github/context/CONTEXT.md` — new entities and relationships
- New ADRs in `.github/decisions/` — for non-obvious decisions

### Phase 2: SPECIFY (Spec Writer)

Takes the informal grilling output and formalizes it:

1. Reads `grilling-output.md`
2. Explores the codebase to verify assumptions
3. Formalizes the domain model (entities, attributes, relationships)
4. Writes user stories with testable acceptance criteria (given/when/then)
5. Documents non-functional requirements and scope boundaries

**Produces:**
- `.github/working/prd.md` — formal Product Requirements Document

### Phase 3: DECOMPOSE (Task Decomposer)

Breaks the PRD into executable tasks:

1. Reads `prd.md`
2. Identifies vertical slices (each issue cuts through ALL layers: DB → API → UI)
3. Makes Issue 1 a "tracer bullet" — the thinnest end-to-end slice
4. Establishes blocking dependencies between issues
5. Assigns agents (BE Developer, FE Developer, or both) per issue

**Produces:**
- `.github/working/issues.md` — ordered tasks with dependencies and agent assignments

### Phase 4: BUILD (Orchestrator + Developers)

The Orchestrator dispatches developers issue by issue:

1. **New chat opened** — Orchestrator reads `issues.md` and `state.yaml`
2. Shows progress summary and proposes next available (unblocked) issues
3. User picks an issue (or accepts the recommendation)
4. Orchestrator dispatches BE Developer and/or FE Developer
5. Developers follow **TDD**: write failing test → implement → refactor → confirm green
6. On completion: Orchestrator updates `state.yaml`, updates `CONTEXT.md`
7. Proposes next issue or waits for new chat

**Produces:**
- Implementation code with passing tests
- `.github/working/state.yaml` — continuously updated progress
- Updated `.github/context/CONTEXT.md` — reflects new entities/routes

### Phase 5: REVIEW (Architecture Reviewer)

After all issues are complete:

1. Reviews the entire batch of changes
2. Runs code-review checklist (naming, boundaries, tests, quality)
3. Assesses architecture (shallow modules, coupling, leaky abstractions)
4. Proposes deepening opportunities (prioritized HIGH/MEDIUM/LOW)
5. Delivers verdict: PASS | PASS WITH SUGGESTIONS | NEEDS REFACTORING

**Produces:**
- `.github/working/review.md` — findings and refactoring suggestions

---

## Agent Roster

| # | Agent | Phase | What It Does | Skills Used |
|---|-------|-------|--------------|-------------|
| 1 | Planner | Understand | Grills user to reach shared understanding | grill-with-docs, ubiquitous-language, adr |
| 2 | Spec Writer | Specify | Formalizes grilling into PRD with user stories | to-prd |
| 3 | Task Decomposer | Decompose | Breaks PRD into vertical-slice issues | — |
| 4 | Orchestrator | Build | Dispatches devs per issue, tracks state | context-update |
| 5 | BE Developer | Build | Implements FastAPI backend using TDD | tdd, code-review |
| 6 | FE Developer | Build | Implements React frontend using TDD | tdd, code-review |
| 7 | Architecture Reviewer | Review | Assesses code quality and structure | improve-architecture, code-review |
| 8 | Requirements | On-demand | Extracts document templates from examples | template-extract |

---

## File Structure

```
.github/
├── copilot-instructions.md              # Entry point (Copilot reads this first)
├── README.md                            # This file
├── state.template.yaml                  # Template for state tracking
│
├── agents/                              # Agent definitions (8 agents)
│   ├── planner.agent.md
│   ├── spec-writer.agent.md
│   ├── task-decomposer.agent.md
│   ├── orchestrator.agent.md
│   ├── be-developer.agent.md
│   ├── fe-developer.agent.md
│   ├── architecture-reviewer.agent.md
│   └── requirements.agent.md
│
├── skills/                              # Reusable instruction sets (9 skills)
│   ├── grill-with-docs.md              # Structured interviewing
│   ├── ubiquitous-language.md          # Glossary maintenance
│   ├── adr.md                          # Decision records
│   ├── context-update.md              # Keep CONTEXT.md in sync
│   ├── to-prd.md                      # Grilling → formal spec
│   ├── tdd.md                         # Test-driven development
│   ├── code-review.md                 # Self-review checklist
│   ├── improve-architecture.md        # Structural assessment
│   └── template-extract.md            # Document template extraction
│
├── rules/                              # Coding & orchestration conventions
│   ├── agent-decisions.md              # Hard orchestration rules
│   ├── rules-architecture.md           # Monorepo layout
│   ├── rules-backend.md               # FastAPI conventions
│   ├── rules-frontend.md              # React conventions
│   ├── rules-sql.md                   # Database conventions
│   ├── rules-testing.md              # Test conventions
│   ├── rules-theming.md              # CSS token system
│   └── rules-i18n.md                 # Internationalization
│
├── context/                            # Living documentation
│   ├── CONTEXT.md                     # System map (entities, relationships, API)
│   └── UBIQUITOUS_LANGUAGE.md        # Shared domain vocabulary
│
├── decisions/                          # Architectural Decision Records
│   ├── ADR-000-template.md
│   └── ADR-001-simplified-agent-system.md
│
├── working/                            # Task artifacts (created during execution)
│   ├── grilling-output.md             # (created by Planner)
│   ├── prd.md                         # (created by Spec Writer)
│   ├── issues.md                      # (created by Task Decomposer)
│   ├── state.yaml                     # (created/updated by Orchestrator)
│   └── review.md                      # (created by Architecture Reviewer)
│
└── templates/                          # Requirements Agent outputs
    └── (extracted templates go here)
```

---

## Key Concepts

### Artifacts and Their Lifecycle

| Artifact | Created By | Location | Persists Between Chats |
|----------|-----------|----------|----------------------|
| `grilling-output.md` | Planner | `.github/working/` | Yes |
| `prd.md` | Spec Writer | `.github/working/` | Yes |
| `issues.md` | Task Decomposer | `.github/working/` | Yes |
| `state.yaml` | Orchestrator | `.github/working/` | Yes (continuously updated) |
| `review.md` | Architecture Reviewer | `.github/working/` | Yes |
| `CONTEXT.md` | Seed → Planner → Orchestrator | `.github/context/` | Yes (living document) |
| `UBIQUITOUS_LANGUAGE.md` | Seed → Planner | `.github/context/` | Yes (living document) |
| ADRs | Planner | `.github/decisions/` | Yes (immutable once written) |

### Ubiquitous Language

A glossary of terms that code, agents, and humans all share. When the glossary says "Service" means "a pure domain function with no HTTP types," then every agent uses that exact definition. This prevents naming drift and ensures AI-generated code matches existing patterns.

### CONTEXT.md

A lightweight system map. Answers: what exists, how things connect, what's the API surface, what's in/out of scope. Agents read this before any work so they don't create duplicates or misunderstand relationships. Updated after each implementation issue completes.

### TDD (Test-Driven Development)

All developer agents follow red-green-refactor:
1. **Red:** Write a failing test that describes desired behavior
2. **Green:** Write minimum code to make it pass
3. **Refactor:** Clean up without changing behavior

This ensures every piece of implementation has a test proving it works.

### Vertical Slicing

Issues cut through ALL layers (database → API → frontend), not horizontal slabs. Issue 1 is always a "tracer bullet" — the thinnest possible end-to-end slice that proves the integration works.

---

## Stack

| Layer | Technology |
|-------|-----------|
| Backend | FastAPI + SQLite (Python) |
| Frontend | React + TypeScript + Vite |
| BE Testing | pytest + TestClient |
| FE Testing | Vitest + React Testing Library |

---

## Getting Started

### For a new feature:

1. Open a chat and invoke the **Planner** — describe what you want to build
2. Answer the grilling questions until shared understanding is reached
3. The **Spec Writer** will produce a PRD
4. The **Task Decomposer** will break it into issues
5. Open a **new chat** and invoke the **Orchestrator**
6. The Orchestrator shows available issues — pick one (or accept its recommendation)
7. Developers implement using TDD
8. Repeat until all issues are done
9. **Architecture Reviewer** assesses the completed work

### Resuming work:

Open a new chat and invoke the Orchestrator. It reads `state.yaml` and shows you where you left off.

---

## On-Demand: Requirements Agent

The Requirements Agent operates outside the normal flow. Invoke it when you have an example requirements document (PDF/Word) from a company and want to extract a reusable template. It produces:
- A markdown template (`.github/templates/<name>.template.md`)
- A YAML schema (`.github/templates/<name>.schema.yaml`)
- A filled example (`.github/templates/<name>.example.md`)
