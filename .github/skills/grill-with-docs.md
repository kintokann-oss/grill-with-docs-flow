# Skill: Grill-with-Docs

## Purpose

Structured interviewing methodology that resolves ambiguity before any code is written. Based on the principle that shared understanding prevents wasted effort.

## When to Use

- Every new feature request (mandatory for the Planner agent)
- When requirements are vague, contradictory, or incomplete
- When new domain concepts are introduced

## Method

### Phase 1: Identify Unknowns

Read the user's request and identify:
- Ambiguous terms (words that could mean multiple things)
- Missing information (what ISN'T stated but matters)
- Assumptions (things you might guess wrong)
- Scope boundaries (what's in vs. out)

### Phase 2: Question Tree

Ask questions that walk down each branch of the design tree. Each question should:
- Be **concrete** (not "what do you want?" but "should X behave as Y or Z?")
- Offer **options** with trade-offs explained
- Include a **recommendation** when you have an informed opinion
- **One question per branch** — don't bundle unrelated concerns

### Phase 3: Challenge Language

For every new term introduced:
1. Check `UBIQUITOUS_LANGUAGE.md` — does a term already exist for this?
2. If yes, use the existing term and explain why
3. If no, propose a precise definition and ask if it captures the intent
4. If there's a collision, surface it explicitly

### Phase 4: Resolve Dependencies

Questions have dependencies. Walk the tree:
```
Q1: What is a Pitch?
  → Q1a: How does it relate to Video? (depends on Q1)
    → Q1a-i: What happens on deletion? (depends on Q1a)
```

Never ask Q1a-i before Q1a is resolved.

### Phase 5: Confirm and Proceed

When all branches are resolved:
1. Summarize decisions made
2. List new/updated terms
3. Identify any ADR-worthy decisions
4. Ask: "Ready to proceed?"

## Anti-Patterns

- **Don't ask too many questions at once** — max 2-3 per message
- **Don't ask questions you can answer from context** — read CONTEXT.md first
- **Don't accept vague answers** — push back with "what specifically do you mean by X?"
- **Don't grill endlessly** — if the user signals urgency, wrap up with what you have

## Output

The grilling session produces:
- Resolved decisions (input to plan creation)
- New vocabulary (input to ubiquitous-language skill)
- Non-obvious decisions (input to adr skill)
