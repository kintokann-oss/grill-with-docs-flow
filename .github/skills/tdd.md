# Skill: Test-Driven Development (TDD)

## Purpose

Red-green loop. This skill is the reference for what a good test is, where tests go, and the rules of the loop.

When exploring the codebase, read `CONTEXT.md` so test names and interface vocabulary match the project's domain language. Respect ADRs in the area you're touching.

## What a Good Test Is

Tests verify behavior through public interfaces, not implementation details. A good test reads like a specification — "user can checkout with valid cart" — and survives refactors because it doesn't care about internal structure.

## Seams — Where Tests Go

A **seam** is the public boundary you test at. Tests live at seams, never against internals.

**Test only at pre-agreed seams.** Before writing any test, confirm the seams with the user. See @.github/skills/codebase-design.md for the vocabulary (module, interface, depth, seam).

## Anti-Patterns

- **Implementation-coupled** — mocks internal collaborators, tests private methods. Tell: the test breaks when you refactor but behavior hasn't changed.
- **Tautological** — the assertion recomputes the expected value the way the code does (`expect(add(a, b)).toBe(a + b)`). Expected values must come from an independent source of truth — a known-good literal, a worked example, the spec.
- **Horizontal slicing** — writing all tests first, then all implementation. Tests verify *imagined* behavior. Work in **vertical slices** instead — one test, one implementation, repeat.

## Rules of the Loop

- **Red before green.** Write the failing test first, then only enough code to pass it.
- **One slice at a time.** One seam, one test, one minimal implementation per cycle.
- **Refactoring is separate.** It belongs to the review stage, not the red-green implementation cycle.

## The Loop

### RED — Write a Failing Test

1. Choose one behavior from the acceptance criteria
2. Write one test (Arrange → Act → Assert)
3. Run the test — confirm it FAILS for the right reason

If the test is hard to write, the interface needs simplifying.

### GREEN — Make It Pass

1. Write the **minimum code** to pass
2. Don't gold-plate, don't anticipate future tests
3. Run the test — confirm it PASSES

### Repeat

Go back to RED with the next behavior. Continue until all acceptance criteria have tests.

## Backend Example

```python
# RED
def test_create_item_returns_201(client, db):
    response = client.post("/api/items", json={"name": "Test", "status": "active"})
    assert response.status_code == 201
    assert response.json()["name"] == "Test"

# GREEN
@router.post("/api/items", status_code=201)
def create_item(data: CreateItemRequest):
    item = item_service.create(data.name, data.status)
    return item
```

## Frontend Example

```tsx
// RED
test('renders item name and status', () => {
  render(<ItemCard name="Test" status="active" />);
  expect(screen.getByText('Test')).toBeInTheDocument();
  expect(screen.getByText('active')).toBeInTheDocument();
});

// GREEN
function ItemCard({ name, status }: ItemCardProps) {
  return <div><span>{name}</span><span>{status}</span></div>;
}
```
