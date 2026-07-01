# Skill: Change Propagation

## Purpose

When any planning artifact is edited, cascade the change to all related artifacts so nothing gets silently out of sync. The Change Propagator MUST announce what it propagated.

## When to Use

- When the user revises a decision after grilling
- When a term is renamed, redefined, or removed
- When an entity or relationship changes
- When a user story is added, removed, or modified
- When a new requirement surfaces or an existing one is dropped
- When an ADR is reversed or superseded

## The Cascade Matrix

### Grilling output changes

When a decision in `grilling-output.md` is revised:

| Check | Update if affected |
|-------|--------------------|
| Did terminology change? | `UBIQUITOUS_LANGUAGE.md` — update definitions |
| Did entities or relationships change? | `CONTEXT.md` — update entities/relationships tables |
| Was a previous ADR reversed? | Create a new ADR superseding the old one |
| Does `prd.md` exist? | Update affected user stories and domain model |
| Does `issues.md` exist? | Update affected issue descriptions and acceptance criteria |

### Entity / domain model changes

When an entity is added, renamed, removed, or its relationships change:

| Artifact | Action |
|----------|--------|
| `UBIQUITOUS_LANGUAGE.md` | Add, rename, or remove the term + definition |
| `CONTEXT.md` | Update the Entities table and Relationships section |
| `grilling-output.md` | Update the Domain Sketch section |
| `prd.md` (if exists) | Update domain model + user stories referencing the entity |
| `issues.md` (if exists) | Update issue descriptions and acceptance criteria referencing the entity |

### Terminology changes

When a term is renamed or redefined in the glossary:

| Artifact | Action |
|----------|--------|
| `UBIQUITOUS_LANGUAGE.md` | Update the definition (source of truth for terms) |
| `grilling-output.md` | Find and replace the old term |
| `CONTEXT.md` | Find and replace the old term |
| `prd.md` (if exists) | Find and replace the old term |
| `issues.md` (if exists) | Find and replace the old term |

### User story changes

When a user story in `prd.md` is added, removed, or modified:

| Artifact | Action |
|----------|--------|
| `issues.md` (if exists) | Add, remove, or update the affected issue(s) and acceptance criteria |
| `CONTEXT.md` | Update boundaries (in-scope / out-of-scope) if scope shifted |
| `issues.md` blocking deps | Re-evaluate if dependency order changed |
| `issues.md` agent assignments | Re-evaluate if scope shifted (BE-only vs BE+FE) |

### Requirement added or removed

When a new requirement surfaces or an existing one is dropped:

| Artifact | Action |
|----------|--------|
| `grilling-output.md` | Add/update the Summary and relevant Grill Log entries |
| `UBIQUITOUS_LANGUAGE.md` | Add new terms if the requirement introduces them |
| `CONTEXT.md` | Update boundaries and entities if affected |
| `prd.md` (if exists) | Add or remove the corresponding user story |
| `issues.md` (if exists) | Add or remove the corresponding issue, re-evaluate deps |

### ADR changes

When an architectural decision is reversed or superseded:

| Artifact | Action |
|----------|--------|
| `.github/decisions/` | Create a new ADR with status "Supersedes ADR-NNN" |
| `grilling-output.md` | Update the ADRs Created section |
| `CONTEXT.md` | Update if the decision affected boundaries or entities |
| `prd.md` (if exists) | Update if the decision affected non-functional requirements or constraints |
| `state.yaml` | Update `adrs_created` list in the understand phase |

## How to Announce

After propagating, tell the user exactly what changed and where:

> "You renamed 'Pitch' to 'Proposal'. Updated:
> - UBIQUITOUS_LANGUAGE.md (definition renamed)
> - CONTEXT.md (entity renamed in Entities table)
> - grilling-output.md (Domain Sketch updated)
> - prd.md (renamed in Stories 2 and 4, updated domain model)
> 
> No changes needed:
> - issues.md — does not reference 'Pitch'
> - ADRs — no ADR references 'Pitch'"

## Rules

- ALWAYS check ALL artifacts in the cascade matrix, not just the obvious ones.
- ALWAYS announce every file you updated and every file you flagged.
- ALWAYS do all updates in one pass — atomic propagation, not spread across multiple interactions.
- NEVER silently skip an artifact — if it doesn't need updating, confirm you checked it.
- NEVER leave orphan references — if you rename a term, catch every occurrence.
- NEVER introduce new terms or concepts during propagation. You propagate existing changes, you don't make new decisions.
- If you're unsure whether a change affects an artifact, READ it and check. Don't guess.
