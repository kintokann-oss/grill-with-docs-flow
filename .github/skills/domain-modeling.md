# Skill: Domain Modeling

## Purpose

Actively build and sharpen the project's domain model. This is the active discipline — challenging terms, inventing edge-case scenarios, and writing the glossary and decisions down the moment they crystallize. Reading `CONTEXT.md` for vocabulary is a one-line habit any skill can do. This skill is for when you're changing the model, not just consuming it.

Used by the Planner (during grilling), the Spec Writer (during PRD formalization), and any agent that discovers a term or relationship that needs sharpening.

## Sub-skills

This skill orchestrates three existing skills:
- @.github/skills/ubiquitous-language.md — glossary maintenance
- @.github/skills/adr.md — architectural decision records
- @.github/skills/context-update.md — keeping CONTEXT.md in sync

## During the Session

### Challenge Against the Glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md` or `UBIQUITOUS_LANGUAGE.md`, call it out immediately: "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

### Sharpen Fuzzy Language

When the user uses vague or overloaded terms, propose a precise canonical term: "You're saying 'account' — do you mean the Customer or the User? Those are different things."

### Discuss Concrete Scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-Reference with Code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code does X, but you just said Y — which is right?"

### Update Inline

When a term is resolved, update `UBIQUITOUS_LANGUAGE.md` and `CONTEXT.md` right there. Don't batch these up — capture them as they happen.

`CONTEXT.md` should be kept thin — a map, not a manual. One line per entity, one line per relationship. No implementation details.

### Offer ADRs Sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR.

## Anti-Patterns

- **Don't treat CONTEXT.md as a spec** — it's a glossary and a lightweight system map, not a requirements document
- **Don't batch glossary updates** — update inline as terms are resolved
- **Don't accept vague answers** — push back with "what specifically do you mean by X?"
- **Don't add ADRs for obvious decisions** — only when it would surprise a future reader
