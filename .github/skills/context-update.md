# Skill: Context Update

## Purpose

Keep `CONTEXT.md` in sync with the actual state of the codebase. The context document is the single source of truth for what the system does, how parts relate, and what boundaries exist.

## When to Use

- After any agent completes implementation work
- After the planner establishes new domain concepts
- When the orchestrator finishes a full task
- When relationships between entities change

## Method

### 1. Identify What Changed

After work is complete, determine:
- Were new entities/models created?
- Were new routes/endpoints added?
- Did relationships between existing concepts change?
- Were new bounded-context boundaries established?

### 2. Update CONTEXT.md

The context document has these sections:

```markdown
# Context

## Entities
(What things exist in the system)

## Relationships
(How entities connect to each other)

## Boundaries
(What is in-scope vs out-of-scope)

## API Surface
(Available endpoints/interfaces)

## Data Model
(Current schema overview)
```

Update ONLY the sections affected by the change. Don't rewrite sections that haven't changed.

### 3. Cross-Reference

After updating:
- Verify terms used in CONTEXT.md match `UBIQUITOUS_LANGUAGE.md`
- Check that no orphan references exist (entities mentioned but never defined)
- Ensure relationships are bidirectional (if A relates to B, B's section mentions A)

### 4. Keep It Thin

CONTEXT.md is a map, not a manual:
- One line per entity
- One line per relationship
- No implementation details (how routes are coded, what libraries are used)
- No duplicating what's obvious from the code structure

## File Location

`.github/context/CONTEXT.md`

## Anti-Patterns

- **Don't duplicate code in the context** — link to files, don't paste implementations
- **Don't let it grow unbounded** — if it exceeds ~100 lines, you're including too much detail
- **Don't update context speculatively** — only update after code actually exists
