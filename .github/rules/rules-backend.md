# Backend Rules

**Scope:** `apps/api/` · **Stack:** FastAPI + SQLite

## File Layout

| Path | Role |
|------|------|
| `src/routes/` | One module per feature — HTTP handlers only |
| `src/services/` | Domain functions — no FastAPI imports |
| `src/db.py` | Connection helpers, `init_db()` |
| `migrations/` | Sorted SQL files applied by `init_db()` |
| `tests/` | Integration tests via TestClient |

## Route Handlers

- One router file per feature (e.g. `items.py`, `users.py`)
- Thin handler: parse request → call service → return response model
- Register routes with `@router.get` / `@router.post` etc.
- Mount router in `main.py`

## Pydantic Models

- Request/response models colocated in the route module (or extracted when shared)
- Use explicit field types; avoid untyped `dict` in public APIs

## Services

- Pure functions: accept primitives/models, return domain data
- No HTTP types (`Request`, `HTTPException`, status codes) inside services
- One table concern per service module when practical

## Persistence

- SQLite via `DATABASE_URL`
- Parameterized queries only (`?` placeholders, never f-strings)
- Schema changes go in migration files, not in service code

## Errors

- Raise `HTTPException` with stable status codes (400, 404, 500)
- Do not leak stack traces or internal paths

## Testing

- Write tests alongside implementation in `tests/`
- Use TestClient for route testing
- Use temp DB file for isolation (set `DATABASE_URL` in fixtures)
- Tests must pass before reporting step complete
