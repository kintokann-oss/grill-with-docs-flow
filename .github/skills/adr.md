# Skill: Architectural Decision Record (ADR)

## Purpose

Document non-obvious decisions so future developers (and AI agents) understand WHY something was done a particular way, not just what was done.

## When to Create an ADR

Only when ALL of the following are true:
- The decision is **hard to reverse** (or costly to reverse)
- The decision would be **surprising without context** (a new developer would ask "why?")
- The decision involved a **real trade-off** with consequences

Do NOT create ADRs for:
- Obvious choices (using React for a React project)
- Easily reversible decisions (which CSS class to use)
- Personal preferences with no trade-offs

## Template

```markdown
# ADR-NNN: [Short Decision Title]

## Status
Accepted | Superseded by ADR-NNN | Deprecated

## Context
What situation or problem prompted this decision? 2-3 sentences max.

## Decision
What did we decide? State it clearly and concisely.

## Consequences

### Positive
- What we gain from this decision

### Negative
- What we lose or accept as a trade-off

### Neutral
- Side effects that are neither good nor bad

## Alternatives Considered
Brief list of what else we could have done and why we didn't.
```

## File Location

`.github/decisions/ADR-NNN-short-title.md`

## Numbering

- Sequential: ADR-001, ADR-002, etc.
- Check existing files in `.github/decisions/` to determine next number

## Method

1. Identify the decision during grilling or implementation
2. Confirm with the user that it's ADR-worthy (hard to reverse? surprising? real trade-off?)
3. Write the ADR using the template above
4. Reference the ADR in `CONTEXT.md` if it affects the bounded context

## Anti-Patterns

- **Don't create ADRs retroactively for everything** — only when someone asks "why?"
- **Don't make ADRs too long** — if it's more than 1 page, split into multiple decisions
- **Don't update old ADRs** — supersede them with a new ADR that references the old one
