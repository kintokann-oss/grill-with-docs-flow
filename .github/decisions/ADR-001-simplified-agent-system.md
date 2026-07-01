# ADR-001: Simplified Agent System Over Full Orchestration Framework

## Status

Accepted (evolved from 5-agent to 9-agent system)

## Context

The original system (`.github2/`) uses 12 specialist agents, YAML profiles with `@profile:` slot resolution, file-based handoff templates, staleness passes, and tiered gates. While powerful, this is too complex for teams to adopt quickly and requires significant ceremony per task.

## Decision

Replace with a streamlined system (currently 9 agents: Planner, Spec Writer, Task Decomposer, Orchestrator, BE Developer, FE Developer, Architecture Reviewer, Requirements, Change Propagator) that uses:
- Markdown skills (reusable instruction files) instead of embedded procedures
- A lightweight YAML state file tracked from Phase 1 onward
- Everything under `.github/` instead of split across `.github/` and `docs/`
- No `@profile:` slot resolution — agents reference paths directly
- Always-grill methodology from grill-with-docs instead of configurable gate tiers

The system evolved from an initial 5-agent design to add Spec Writer, Task Decomposer, Architecture Reviewer, and Change Propagator as separation of concerns proved valuable.

## Consequences

### Positive
- Faster onboarding for new team members
- Less ceremony per task (no handoff templates to fill)
- Easier to understand and modify
- Portable — any team can drop this into a FastAPI+React repo
- Architecture Reviewer provides formal validation (Phase 5)
- Change Propagator prevents silent drift between planning artifacts

### Negative
- Less granular control (no separate testing agents, no navigator)
- No staleness detection — context can drift if context-update skill is skipped

### Neutral
- Same documentation philosophy (CONTEXT.md, ubiquitous language, ADRs)
- Same grill-with-docs methodology for planning

## Alternatives Considered

- Keep the full 12-agent system and just improve docs (rejected: complexity is the problem, not documentation)
- Go even simpler with 2 agents (rejected: loses the planning/doing separation that prevents wasted work)
