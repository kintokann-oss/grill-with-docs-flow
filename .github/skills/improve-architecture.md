# Skill: Improve Codebase Architecture

## Purpose

Systematically identify structural problems in the codebase that make it harder for agents (and humans) to understand, test, and extend. Proposes concrete deepening opportunities.

## When to Use

- After completing all issues in a task (Architecture Reviewer phase)
- Periodically (weekly or after a surge of development)
- When developers report confusion navigating the codebase

## Method

### 1. Explore Naturally

Don't follow a checklist mechanically. Read the code as a newcomer would:
- Start at entry points (`main.py`, `App.tsx`)
- Follow the call chain for a typical request
- Notice where you get confused, lost, or have to bounce between files

### 2. Identify Confusions

Look for these patterns:

**Shallow modules (too many tiny files):**
- A concept requires reading 5+ files to understand
- Functions are extracted into separate files "for organization" but are only called once
- A folder has 15 files averaging 20 lines each

**False testability extractions:**
- Pure functions extracted just so they can be unit-tested in isolation
- But the real bugs are in how they're combined (integration boundaries)
- Tests pass but the feature is broken

**Tight coupling masquerading as separation:**
- Module A imports 5 things from Module B
- Changes to B always require changes to A
- They're in separate files but might as well be one

**Leaky abstractions:**
- Callers need to understand implementation details to use a module correctly
- Error handling requires knowledge of the underlying library
- Configuration bleeds through interfaces

### 3. Propose Deepening Opportunities

For each problem, suggest ONE concrete improvement:

| Problem Type | Typical Solution |
|-------------|-----------------|
| Too many tiny files | Merge into one deeper module with a thin public interface |
| False extraction | Inline back into the caller; test at the integration boundary instead |
| Tight coupling | Extract a shared interface, or merge the coupled modules |
| Leaky abstraction | Wrap with a facade that hides the complexity |

### 4. Prioritize

Rate each opportunity:
- **HIGH:** Structural risk — will cause bugs or make future features harder
- **MEDIUM:** Quality — makes code harder to understand but isn't immediately dangerous
- **LOW:** Style — could be better but works fine as-is

### 5. Present Options

For HIGH priority items, present 2-3 alternative designs with trade-offs:
- Option A: [description] — Pros: ... Cons: ...
- Option B: [description] — Pros: ... Cons: ...
- Recommended: [which one and why]

## Key Principle: Deep Modules

> "The best modules are those that provide powerful functionality yet have simple interfaces." — John Ousterhout, A Philosophy of Software Design

A **deep module** is:
- Simple to use (thin interface: few params, clear purpose)
- Complex inside (hides real-world messiness)
- Self-contained (doesn't leak implementation)

A **shallow module** is:
- Complex to use (many params, need to understand internals)
- Simple inside (just delegates or reorganizes)
- Leaky (callers must know implementation details)

**The goal is not fewer files. The goal is clearer boundaries.**

## Anti-Patterns

- **Don't refactor everything** — only propose changes for real confusions, not theoretical purity
- **Don't break working code** — proposals must preserve existing behavior
- **Don't ignore the team** — if a pattern is well-understood by the team (even if "ugly"), that's OK
- **Don't optimize for AI** — optimize for human understanding; AI benefits as a side effect
