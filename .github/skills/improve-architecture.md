# Skill: Improve Codebase Architecture

## Purpose

Surface architectural friction and propose deepening opportunities — refactors that turn shallow modules into deep ones. The aim is testability and AI-navigability.

## When to Use

- After completing all issues in a task (Architecture Reviewer phase)
- Periodically (weekly or after a surge of development)
- When developers report confusion navigating the codebase

## Process

### 1. Read Context First

Read `CONTEXT.md` for the domain glossary and any ADRs in the area you're touching. Use domain vocabulary for everything — if `CONTEXT.md` defines "Order," talk about "the Order module," not "the FooBarHandler."

### 2. Explore Naturally

Walk the codebase organically. Note where you experience friction:

- Where does understanding one concept require bouncing between many small modules?
- Where are modules **shallow** — interface nearly as complex as the implementation?
- Where have pure functions been extracted just for testability, but the real bugs hide in how they're called?
- Where do tightly-coupled modules leak across their seams?

Apply the **deletion test** (from @.github/skills/codebase-design.md): would deleting a module concentrate complexity, or just move it?

### 3. Present Candidates

For each candidate, provide:

- **Files** — which modules are involved
- **Problem** — why the current architecture causes friction
- **Solution** — what would change
- **Benefits** — in terms of locality and leverage
- **Recommendation strength** — `Strong`, `Worth exploring`, or `Speculative`

End with a **top recommendation**: which candidate to tackle first and why.

**ADR conflicts**: if a candidate contradicts an existing ADR, only surface it when the friction is real enough to warrant revisiting. Don't list every theoretical refactor an ADR forbids.

Do NOT propose interfaces yet. Ask the user: "Which of these would you like to explore?"

### 4. Grill Through the Chosen Candidate

Once the user picks one, grill through it together:
- Constraints and dependencies
- The shape of the deepened module
- What sits behind the seam
- What tests survive the refactor

Update `CONTEXT.md` and offer ADRs as decisions crystallize during the conversation.

## Key Principle: Deep Modules

See @.github/skills/codebase-design.md for the full vocabulary (module, interface, depth, seam, adapter, leverage, locality).

**The goal is not fewer files. The goal is clearer boundaries.**

## Anti-Patterns

- **Don't refactor everything** — only propose changes for real confusions
- **Don't break working code** — proposals must preserve existing behavior
- **Don't ignore the team** — if a pattern is well-understood (even if "ugly"), that's OK
