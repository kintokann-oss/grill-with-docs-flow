# Skill: Ubiquitous Language

## Purpose

Maintain a shared vocabulary that the codebase, developers, and domain experts all use consistently. Prevents the same concept from having multiple names and ensures AI agents use precise terminology.

## When to Use

- After a grilling session surfaces new terms
- When a code review reveals inconsistent naming
- When a new domain concept is introduced
- When an existing term's meaning shifts

## Method

### 1. Check Before Adding

Before introducing a new term:
- Read `UBIQUITOUS_LANGUAGE.md` — does a synonym already exist?
- Search the codebase — is the concept already named differently in code?
- If collision exists, resolve it (rename code or adjust the glossary)

### 2. Define Precisely

Each term entry must have:
- **Term:** The canonical name (PascalCase for types, snake_case for fields)
- **Definition:** One sentence, unambiguous
- **Relationships:** How it connects to other terms (if applicable)
- **NOT:** What it explicitly is NOT (if confusion is likely)

Format:
```markdown
### Term Name

**Definition:** One clear sentence.

**Relationships:** References to related terms.

**Not:** What this is NOT (to prevent confusion).
```

### 3. Validate Against Code

After updating the glossary:
- The term MUST match what's used in code (variable names, table names, route names)
- If code uses a different name, either rename the code or the term
- Never let the glossary drift from the codebase

### 4. Cascade Updates

When a term changes:
- Update `UBIQUITOUS_LANGUAGE.md`
- Update `CONTEXT.md` if the term appears there
- Flag code that needs renaming (but don't rename in this skill — that's a dev task)

## File Location

`.github/context/UBIQUITOUS_LANGUAGE.md`

## Anti-Patterns

- **Don't add obvious terms** — only terms where confusion is possible
- **Don't add implementation details** — "we use PostgreSQL" isn't vocabulary
- **Don't let definitions grow** — if a definition needs a paragraph, it might be two concepts
