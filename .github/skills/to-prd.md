# Skill: To PRD (Product Requirements Document)

## Purpose

Transform grilling output into a formal PRD. Defines WHAT to build with testable acceptance criteria. Do NOT re-interview — synthesize what's already been discussed.

## When to Use

- After the Planner completes grilling and produces `grilling-output.md`

## Process

1. **Explore the repo** to confirm assertions from grilling. Use domain glossary vocabulary throughout. Respect ADRs in the area you're touching.

2. **Sketch testing seams** — where will this feature be tested? Prefer existing seams. Use the highest seam possible. Check with the user that these match their expectations.

3. **Write the PRD** using the template below.

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
[If applicable: states + transitions]

## User Stories

A long, numbered list. Each story:

1. As a [role], I want to [action], so that [benefit]

   **Acceptance criteria:**
   - Given [context], when [action], then [result]

This list should be extensive and cover all aspects of the feature.

## Implementation Decisions

- Modules that will be built/modified
- Interfaces that will change
- Schema changes
- API contracts
- Testing seams agreed upon

Do NOT include specific file paths or code snippets — they go stale fast.

## Testing Decisions

- What makes a good test for this feature (behavior, not implementation)
- Which modules/seams will be tested
- Prior art (similar tests in the codebase)

## Out of Scope
- [explicitly stated boundaries]
```

## Anti-Patterns

- **Don't specify HOW** — PRDs describe behavior, not implementation
- **Don't be vague** — "item should work properly" is useless; be specific
- **Don't add unrequested features** — only document what was agreed during grilling
- **Don't skip edge cases** — if grilling identified them, they belong in acceptance criteria
