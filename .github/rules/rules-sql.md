# SQL / Persistence Rules

**Scope:** `apps/api/migrations/`, `apps/api/src/db.py` · **Engine:** SQLite

## Stack

| Piece | Location |
|-------|----------|
| Engine | SQLite via stdlib `sqlite3` |
| Connection | `apps/api/src/db.py` — `get_connection()`, `get_database_path()`, `init_db()` |
| Migrations | `apps/api/migrations/*.sql` — applied in sorted filename order by `init_db()` |
| Env override | `DATABASE_URL` |

## Migration Rules

1. **One file per change** — `NNN_description.sql` (zero-padded prefix, snake_case)
2. **Idempotent** — prefer `CREATE TABLE IF NOT EXISTS`, `INSERT OR IGNORE`
3. **No DROP** unless explicitly required (document why)
4. **No raw DDL in routes or services** — only in migration files
5. Migrations are applied in sorted filename order

## Query Rules

- Parameterized queries only — `?` placeholders, never f-string SQL
- One table concern per service module when practical
- Shared constants (state keys, enums) live in `db.py`

## Testing

- Tests set `DATABASE_URL` to a temp file for isolation
- Schema is created fresh per test via `init_db()`
- No DDL in test code — tests rely on migrations
