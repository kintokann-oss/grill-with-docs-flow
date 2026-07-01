# Skill: Code Review

## Purpose

Two-axis review of code changes. Checks both **Standards** (does the code follow this repo's documented coding standards?) and **Spec** (does the code match what the originating issue/PRD asked for?). A change can pass one axis and fail the other — reporting them separately stops one from masking the other.

## When to Use

- After the BE Developer completes implementation
- After the FE Developer completes implementation
- Before any agent reports a step as "done"
- When reviewing a branch or PR

## Axis 1: Standards

Does the code conform to this repo's documented coding standards?

### Naming & Language

- [ ] All new variables, functions, classes use terms from `UBIQUITOUS_LANGUAGE.md`
- [ ] No synonyms or abbreviations that conflict with established vocabulary
- [ ] File names follow project conventions (kebab-case for files, PascalCase for components)

### Architecture

- [ ] Code is in the correct directory per `rules-architecture.md`
- [ ] No boundary violations (BE code in FE directories or vice versa)
- [ ] New modules follow existing patterns in the same directory
- [ ] Modules are deep, not shallow — use `codebase-design` vocabulary

### Implementation Quality

- [ ] No hardcoded values that should be configuration
- [ ] Error handling is present for all failure modes
- [ ] No unused imports or dead code
- [ ] Functions have a single responsibility
- [ ] No obvious performance issues (N+1 queries, unnecessary re-renders)

### Testing

- [ ] Tests exist for the new code
- [ ] Tests cover the happy path and at least one error case
- [ ] Tests use descriptive names that explain the scenario
- [ ] Tests verify behavior through public interfaces, not implementation details
- [ ] Tests pass locally

### Stack-Specific (Backend)

- [ ] Routes follow RESTful conventions
- [ ] Database queries use parameterized statements
- [ ] Migrations are reversible where possible
- [ ] Response schemas are consistent with existing endpoints

### Stack-Specific (Frontend)

- [ ] Components are accessible (semantic HTML, ARIA where needed)
- [ ] User-facing text uses i18n keys (not hardcoded strings)
- [ ] Styles follow the theme system (`rules-theming.md`)
- [ ] No inline styles that should be theme tokens

### Smell Baseline

Check for these code smells in the changed code:

- **Mysterious Name** — name doesn't reveal what it does. Fix: rename; if no honest name comes, the design is murky.
- **Feature Envy** — a method that reaches into another object's data more than its own. Fix: move the method.
- **Data Clumps** — same fields keep travelling together (a type wanting to be born). Fix: bundle into one type.
- **Primitive Obsession** — a string standing in for a domain concept. Fix: give the concept its own type.
- **Shotgun Surgery** — one logical change scattered across many files. Fix: gather what changes together.
- **Speculative Generality** — abstraction added for needs the spec doesn't have. Fix: delete it.
- **Middle Man** — a class that mostly just delegates. Fix: cut it, call the real target.

## Axis 2: Spec

Does the code match what the originating issue/PRD asked for?

### Locate the Spec

Find the originating spec in this order:
1. The issue from `issues.md` that this work addresses
2. The corresponding user story in `prd.md`
3. If neither exists, note "no spec available" and skip this axis

### Check Against Spec

- [ ] **Missing requirements** — things the spec asked for that aren't implemented or are partial
- [ ] **Scope creep** — behavior in the code that wasn't asked for by the spec
- [ ] **Wrong implementation** — requirements that look implemented but the implementation doesn't match the acceptance criteria

For each finding, quote the spec line (user story or acceptance criterion) it relates to.

## Reporting

Present the two axes separately:

```
## Standards
- [PASS/ISSUE] Naming & Language: ...
- [PASS/ISSUE] Architecture: ...
- ...

## Spec
- [PASS/MISSING] Story 1 criterion 2: ...
- [PASS/WRONG] Story 3 criterion 1: ...
- [SCOPE CREEP] Feature X was not in the spec
```

End with a one-line summary: total findings per axis.

## Anti-Patterns

- **Don't rubber-stamp** — actually check each item
- **Don't merge the two axes** — a change can pass Standards and fail Spec (or vice versa)
- **Don't block on style preferences** — only flag violations of documented rules
- **Don't rewrite working code** — fix only items that fail the checklist
- **Smell items are always judgment calls** — not hard violations
