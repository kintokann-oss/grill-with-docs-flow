# Skill: To PRD (Product Requirements Document)

## Purpose

Transform grilling output (informal shared understanding) into a formal, structured Product Requirements Document that clearly defines WHAT to build with testable acceptance criteria.

## When to Use

- After the Planner completes grilling and produces `grilling-output.md`
- Used by the Spec Writer agent

## Method

### 1. Skip Redundant Steps

If grilling is already complete, do NOT re-interview. Start from the grilling output directly. Only ask follow-up questions if something is genuinely unclear or contradictory.

### 2. Verify Against Code

Before writing the PRD:
- Explore the codebase to confirm assertions from grilling
- Check what exists vs what needs to be created
- Note existing patterns to extend (don't reinvent)

### 3. Formalize Domain Model

Take informal entity sketches from grilling and formalize:
- Entity name (from UBIQUITOUS_LANGUAGE.md)
- Attributes with types
- Relationships with cardinality (1:1, 1:N, N:N)
- State machines (if lifecycle states exist)

### 4. Write User Stories

For EVERY behavior:

```markdown
### Story: [Short descriptive title]

As a [user role],
I want to [action/capability],
So that [benefit/value].

**Acceptance criteria:**
- Given [initial context], when [action taken], then [expected result]
- Given [context], when [action], then [result]
```

Rules for good acceptance criteria:
- Each criterion is independently testable
- Uses concrete values, not vague ("Given item with status 'active'" not "Given some item")
- Covers happy path AND error/edge cases
- References domain terms from the glossary

### 5. Non-Functional Requirements

Document constraints that aren't user stories:
- Data validation rules
- Error handling behavior
- Performance expectations (if discussed)
- i18n requirements
- Accessibility needs

### 6. Scope Boundary

Explicitly state what is OUT of scope — prevents scope creep during implementation.

## Template

```markdown
# PRD: [Feature Title]

## Overview
[1 paragraph: what this feature does and why it matters]

## Domain Model

### Entities
| Entity | Attributes | Type | Required |
|--------|-----------|------|----------|

### Relationships
| From | To | Cardinality | Description |
|------|-----|-------------|-------------|

### State Machines
[If applicable: states + transitions diagram]

## User Stories

### Story 1: [title]
As a...
Acceptance criteria:
- ...

### Story 2: [title]
...

## Non-Functional Requirements
- ...

## Out of Scope
- ...
```

## Anti-Patterns

- **Don't specify HOW** — PRDs describe behavior, not implementation
- **Don't be vague** — "item should work properly" is useless; be specific
- **Don't add unrequested features** — only document what was agreed during grilling
- **Don't skip edge cases** — if grilling identified them, they belong in acceptance criteria
