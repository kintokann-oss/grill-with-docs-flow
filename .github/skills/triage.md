# Skill: Triage

## Purpose

Move issues through a state machine of triage roles — categorize, verify, grill if needed, and produce agent-ready briefs. Use when issues pile up and need evaluation before work begins.

## When to Use

- When bug reports or feature requests arrive
- When the backlog needs evaluation
- When an issue needs to be assessed before entering the planning flow

## Roles

Two **category** roles:

| Role | Meaning |
|------|---------|
| `bug` | Something is broken |
| `enhancement` | New feature or improvement |

Five **state** roles:

| Role | Meaning |
|------|---------|
| `needs-triage` | Needs evaluation |
| `needs-info` | Waiting on reporter for more information |
| `ready-for-agent` | Fully specified, ready for an agent to pick up |
| `ready-for-human` | Needs human implementation (judgment calls, design decisions) |
| `wontfix` | Will not be actioned |

Every triaged issue should carry exactly one category role and one state role.

## State Machine

```
unlabeled → needs-triage → needs-info → (reporter replies) → needs-triage
                         → ready-for-agent
                         → ready-for-human
                         → wontfix
```

## Process

### 1. Show What Needs Attention

Present three buckets, oldest first:

1. **Unlabeled** — never triaged
2. **needs-triage** — evaluation in progress
3. **needs-info with new activity** — reporter replied, needs re-evaluation

Show counts and a one-line summary per item.

### 2. Triage a Specific Issue

**Gather context:**
- Read the full issue (body, comments, labels)
- Explore the codebase using domain glossary terms from `CONTEXT.md`
- Check for **redundancy** — search for an existing implementation of the requested behavior
- Check for **prior rejection** — has a similar request been rejected before?

**Recommend:** Tell the maintainer your category + state recommendation with reasoning.

**Verify the claim:**
- For a bug: try to reproduce from the reporter's steps
- For an enhancement: confirm it doesn't already exist

**Grill if needed:** If the request needs fleshing out, use the domain-modeling skill to sharpen terms and walk the design tree.

### 3. Apply the Outcome

**ready-for-agent** — Write an agent brief:

```markdown
## What to Build
A concise description of the vertical slice. Describe end-to-end behavior.

## Acceptance Criteria
- [ ] Criterion 1
- [ ] Criterion 2

## Blocked By
None — or reference to blocking issue.
```

**ready-for-human** — Same format, but note why it can't be delegated.

**needs-info** — Post triage notes:

```markdown
## Triage Notes

**What we've established so far:**
- point 1
- point 2

**What we still need from you:**
- question 1
- question 2
```

**wontfix** — Close with a polite explanation.

## Resuming a Previous Session

If prior triage notes exist on the issue, read them, check whether the reporter has answered outstanding questions, and present an updated picture before continuing.

## Rules

- ALWAYS categorize (bug vs enhancement) AND assign a state.
- ALWAYS check for existing implementations before recommending work.
- ALWAYS use domain glossary terms from `CONTEXT.md` in issue titles and descriptions.
- NEVER skip verification — reproduce bugs, confirm enhancements don't exist.
- Questions in needs-info must be specific and actionable, not "please provide more info."
