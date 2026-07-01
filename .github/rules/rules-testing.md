# Testing Rules

## Stack

| Layer | Tool |
|-------|------|
| Backend | pytest + FastAPI TestClient |
| Frontend | Vitest + React Testing Library + jsdom |
| E2E | Out of scope unless plan adds it |

## Placement

Tests are colocated with source:

| Source | Test File |
|--------|-----------|
| `components/Foo/Foo.tsx` | `components/Foo/Foo.test.tsx` |
| `hooks/useBar.ts` | `hooks/useBar.test.ts` |
| `api/client.ts` | `api/client.test.ts` |
| Backend routes/services | `tests/test_<feature>.py` |

Do not create separate `__tests__/` directories or repo-root test mirrors.

## Naming

- `Foo.tsx` → `Foo.test.tsx`
- `useBar.ts` → `useBar.test.ts`
- Backend features → `tests/test_<feature>.py`

## AAA Pattern (mandatory)

Every test uses Arrange → Act → Assert structure.

## Frontend i18n in Tests

- Use `i18n.t('namespace:key')` for expected text
- Do not hardcode user-facing strings in assertions

## Commands

- Backend: `cd apps/api && python -m pytest`
- Frontend: `cd apps/web-react && npm test`

## Anti-Patterns

- Hardcoded locale strings in FE test assertions
- Testing implementation details instead of observable behavior
- Marking a step done while tests fail
