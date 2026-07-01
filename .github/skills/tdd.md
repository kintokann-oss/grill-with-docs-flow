# Skill: Test-Driven Development (TDD)

## Purpose

Force a red-green-refactor loop that produces implementation backed by tests at every step. Tests are written FIRST, ensuring code is designed for testability and every behavior has coverage.

## When to Use

- Every time a developer agent implements a feature (mandatory)
- Every acceptance criterion gets its own TDD cycle

## The Loop

### Phase 1: RED — Write a Failing Test

Before writing ANY implementation:

1. **Choose one behavior** from the acceptance criteria
2. **Design the interface** — what function/component will be called? What are its inputs and outputs?
3. **Write one test** that describes this behavior:
   - Use AAA pattern (Arrange → Act → Assert)
   - Test the WHAT (observable behavior), not the HOW (implementation details)
   - The test should be specific enough that there's only one way to make it pass

4. **Run the test** — confirm it FAILS
5. **Verify failure reason** — it should fail because the feature doesn't exist yet, NOT because of a syntax error or bad import

If the test is hard to write:
- The interface is too complex → **simplify the interface first**
- You're testing too much at once → **break into smaller behaviors**
- You need to mock too many things → **the module has too many dependencies**

### Phase 2: GREEN — Make It Pass

1. Write the **minimum code** to make the failing test pass
2. Don't gold-plate — ugly code that passes is fine at this stage
3. Don't write code for behaviors that aren't tested yet
4. **Run the test** — confirm it PASSES

If it still fails:
- Read the error carefully
- Fix only what's needed for this specific test
- Don't refactor yet

### Phase 3: REFACTOR — Clean Up

1. Look at the code you just wrote — is there duplication? Unclear naming? Unnecessary complexity?
2. Apply improvements WITHOUT changing behavior
3. **Run ALL tests** — confirm everything still passes
4. If any test breaks, your refactoring changed behavior — revert and try again

### Repeat

Go back to RED with the next behavior. Continue until all acceptance criteria have tests.

## Interface Design Principles

### Deep Modules Over Shallow

A good module:
- Has a **simple interface** (few parameters, clear purpose)
- **Hides complexity** (the caller doesn't need to know how it works)
- Is **easy to test** (simple inputs, observable outputs)

A bad module:
- Requires understanding its internals to use
- Has many parameters or complex configuration
- Forces callers to handle its internal concerns

### Testability Signals

If you find yourself needing to:
- Mock 5+ dependencies → module has too many responsibilities
- Set up complex state before testing → interface is too coupled to context
- Test private methods → extract them into a separate module with a public interface
- Write "integration tests" for a single function → function does too much

## Backend TDD Example

```python
# RED: Write failing test
def test_create_item_returns_201(client, db):
    response = client.post("/api/items", json={"name": "Test", "status": "active"})
    assert response.status_code == 201
    assert response.json()["name"] == "Test"

# GREEN: Minimum implementation
@router.post("/api/items", status_code=201)
def create_item(data: CreateItemRequest):
    item = item_service.create(data.name, data.status)
    return item

# REFACTOR: (if needed after multiple tests)
```

## Frontend TDD Example

```tsx
// RED: Write failing test
test('renders item name and status', () => {
  render(<ItemCard name="Test" status="active" />);
  expect(screen.getByText('Test')).toBeInTheDocument();
  expect(screen.getByText('active')).toBeInTheDocument();
});

// GREEN: Minimum implementation
function ItemCard({ name, status }: ItemCardProps) {
  return <div><span>{name}</span><span>{status}</span></div>;
}

// REFACTOR: Add proper styling, semantic HTML, etc.
```

## Anti-Patterns

- **Writing tests after implementation** — defeats the purpose; you're testing what you wrote, not what should exist
- **Writing multiple tests before any implementation** — you lose the feedback loop
- **Skipping the RED step** — if the test passes immediately, it's testing nothing useful
- **Refactoring during GREEN** — resist the urge; get to green first, then clean up
- **Testing implementation details** — test behavior (what the user/caller sees), not internals
