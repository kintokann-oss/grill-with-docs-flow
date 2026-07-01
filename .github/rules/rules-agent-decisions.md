# Agent Decisions

Hard rules that all agents follow without asking the user.

## Agent Separation

| Concern | Owner | Phase |
|---------|-------|-------|
| Grill user + shared language + ADRs | Planner | Understand |
| Formal PRD + domain model | Spec Writer | Specify |
| Vertical-slice decomposition | Task Decomposer | Decompose |
| Dispatch + state tracking | Orchestrator | Build |
| Backend code (TDD) | BE Developer | Build |
| Frontend code (TDD) | FE Developer | Build |
| Architecture assessment | Architecture Reviewer | Review |
| Template extraction (on-demand) | Requirements | — |
| Cascade changes across all planning artifacts | Change Propagator | Cross-phase |

No agent crosses into another agent's concern.

## Minimal Reads (Context Budget)

Agents ONLY load files relevant to their current step. Do NOT load all rules, all context, or all skills upfront.

| Principle | Rule |
|-----------|------|
| **Load on need** | Only read a file when the current task requires information from it |
| **Skip irrelevant rules** | If the issue is backend-only, do NOT load frontend rules (theme-rules, i18n, react-best-practices, react-hooks-and-effects-best-practices) |
| **Skip irrelevant skills** | Only invoke a skill when the process calls for it (e.g., don't load TDD skill during planning) |
| **Context first, details later** | Read CONTEXT.md and UBIQUITOUS_LANGUAGE.md first (lightweight). Only read specific rules when you're about to write code that falls under them |
| **No full scans** | Never read every file in a directory "just in case" |

**Per-agent guidance:**
- **Planner:** Load context + vocabulary + existing ADRs. Do NOT load coding rules.
- **Spec Writer:** Load grilling-output + context + vocabulary. Do NOT load coding rules.
- **Task Decomposer:** Load PRD + context + architecture rules only.
- **Orchestrator:** Load issues.md + state.yaml + context. Do NOT load coding rules.
- **BE Developer:** Load issue + context + vocabulary + backend/sql/testing/clean-code rules. Skip frontend rules entirely.
- **FE Developer:** Load issue + context + vocabulary + frontend/theme-rules/react-best-practices/react-hooks/i18n/testing/clean-code rules. Skip backend/sql rules entirely.
- **Architecture Reviewer:** Load context + architecture rules + codebase-design + what was changed. Load other rules only when checking specific violations.
- **Change Propagator:** Load change-propagation skill + affected artifacts + state.yaml. Do NOT load coding rules.
- **Requirements:** Load template-extract skill + example document only.

## Phase Flow (Mandatory Order)

```
UNDERSTAND → SPECIFY → DECOMPOSE → BUILD → REVIEW
```

| Phase | Agent | Input | Output |
|-------|-------|-------|--------|
| Understand | Planner | User request | grilling-output.md, CONTEXT.md updates, UBIQUITOUS_LANGUAGE.md updates, ADRs |
| Specify | Spec Writer | grilling-output.md | prd.md |
| Decompose | Task Decomposer | prd.md | issues.md |
| Build | Orchestrator + Devs | issues.md | Implementation + passing tests |
| Review | Architecture Reviewer | Completed codebase | review.md |

## Grill-with-Docs (Phase 1 — Mandatory)

Before any specification is written:

1. **Challenge fuzzy language** — map user words to glossary terms or propose new definitions
2. **Surface term collisions** — one term, one meaning
3. **Concrete scenarios** — given/when/then for edge cases
4. **Cross-reference existing code** — cite existing symbols before proposing new ones
5. **ADRs** — when a decision is hard to reverse, surprising, or a real trade-off
6. **Update glossary** — add or sharpen terms in `UBIQUITOUS_LANGUAGE.md`

Ask in batches of 1–3 questions. Continue until design branches are resolved or the user says to proceed.

**Ask about:** behavior, UX, data meaning, extend vs replace, cardinality, deletion/empty states.
**Do NOT ask about:** framework choice, test runner, agent routing, file structure.

## PRD (Phase 2)

- Spec Writer formalizes grilling output into testable user stories
- Every behavior needs acceptance criteria (given/when/then)
- PRD describes WHAT, never HOW
- Verify assertions against actual codebase before writing

## Vertical Slicing (Phase 3)

- Issue 1 is ALWAYS a tracer bullet (thinnest end-to-end slice)
- Issues cut through all layers (DB → API → UI), never horizontal slabs
- Each issue has explicit blocking dependencies
- Each issue assigns responsible agent(s)

## TDD (Phase 4 — Mandatory for Developers)

All developer agents follow red-green-refactor:
1. **Red:** Write failing test for one behavior
2. **Green:** Write minimum code to pass
3. **Refactor:** Clean up, re-run tests

- NEVER write implementation before the test
- If a test is hard to write, the interface needs simplifying
- All tests must pass before an issue is marked done

## State Tracking

State tracking begins in Phase 1. The **Planner creates `state.yaml`** at the start of grilling, not the Orchestrator.

- **Phase 1 (Understand):** Planner creates `state.yaml` with `current_phase: understand` and tracks sub-steps (grilling status, glossary/context/ADR updates)
- **Phase 2-3:** Each agent updates its phase status to `in_progress` on start and `done` on completion
- **Phase 4 (Build):** Orchestrator adds the issues section and tracks per-issue status (pending/in_progress/done/failed)
- **Every new chat:** The active agent reads `state.yaml` first to know where things stand

## Change Propagation (Mandatory)

When any planning artifact is edited and the change could ripple to other artifacts, the editing agent MUST invoke the **Change Propagator** agent (`.github/agents/change-propagator.agent.md`).

The Change Propagator:
- Owns the full cascade matrix — knows every artifact dependency
- Updates everything affected in one atomic pass
- Announces every file it updated and every file it checked
- Is the ONLY agent that should propagate changes across multiple artifacts

Other agents do NOT propagate changes themselves. They make their own edit, then delegate to the Change Propagator for the ripple.

## Failure Handling

If any agent's step fails:
1. Orchestrator marks the issue as `failed` in state.yaml
2. Orchestrator reports to user with what went wrong
3. User decides: retry, skip, or abort

## Architecture Review (Phase 5)

After all issues complete:
- Architecture Reviewer assesses structural quality
- Identifies shallow modules, coupling, leaky abstractions
- Proposes deepening opportunities (prioritized HIGH/MEDIUM/LOW)
- Verdict: PASS | PASS WITH SUGGESTIONS | NEEDS REFACTORING

## Context Updates

After completing each issue, the Orchestrator triggers the context-update skill to keep `CONTEXT.md` in sync.

## On-Demand Agents

The **Requirements Agent** operates outside the normal flow. Users invoke it explicitly for template extraction work. It does not participate in the phase pipeline.
