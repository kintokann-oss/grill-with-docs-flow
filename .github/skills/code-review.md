# Skill: Code Review

## Purpose

Standardized self-review checklist that developer agents run against their own output before reporting completion. Catches common issues before they reach the orchestrator.

## When to Use

- After the BE Developer completes implementation
- After the FE Developer completes implementation
- Before any agent reports a step as "done"

## Checklist

### Naming & Language

- [ ] All new variables, functions, classes use terms from `UBIQUITOUS_LANGUAGE.md`
- [ ] No synonyms or abbreviations that conflict with established vocabulary
- [ ] File names follow project conventions (kebab-case for files, PascalCase for components)

### Architecture

- [ ] Code is in the correct directory per `rules-architecture.md`
- [ ] No boundary violations (BE code in FE directories or vice versa)
- [ ] New modules follow existing patterns in the same directory

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

### Documentation

- [ ] Non-obvious logic has a brief comment explaining WHY (not what)
- [ ] Public API functions have clear parameter descriptions
- [ ] No commented-out code left behind

## Reporting

After running the checklist, report:
- **Pass:** All items checked, ready for orchestrator
- **Issues found:** List specific items that failed with brief explanation
- **Fixed:** If you fixed issues yourself, note what was changed

## Anti-Patterns

- **Don't rubber-stamp** — actually check each item
- **Don't block on style preferences** — only flag violations of documented rules
- **Don't rewrite working code** — fix only items that fail the checklist
