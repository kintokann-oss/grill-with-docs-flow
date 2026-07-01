# Context

> **What is this file?**
> This is the bounded context document — a living map of your system. It answers:
> "What things exist? How do they connect? What's the API surface? What's in/out of scope?"
>
> Agents read this BEFORE doing any work so they don't reinvent things that already exist
> or misunderstand how pieces relate.
>
> **Who updates it?**
> - **Planner** (Phase 1): adds new entities/relationships discovered during grilling
> - **Orchestrator** (Phase 4): triggers context-update skill after each issue completes,
>   adding newly implemented entities, routes, and relationships
>
> **Keep it thin.** This is a map, not a manual. One line per entity, one line per relationship.
> No implementation details. If it exceeds ~100 lines, you're including too much.

---

## Entities

| Entity | Location | Description |
|--------|----------|-------------|
| App | `apps/api/src/main.py` | FastAPI application entry point |
| Health | `apps/api/src/routes/health.py` | Liveness endpoint |
| Info | `apps/api/src/routes/info.py` | App name/version endpoint |
| Database | `apps/api/src/db.py` | SQLite connection + migration runner |
| WebApp | `apps/web-react/src/App.tsx` | React root shell |
| InfoClient | `apps/web-react/src/api/info.ts` | GET `/api/info` client |

## Relationships

- WebApp → InfoClient → Info (FE calls BE for app metadata)
- App mounts Health and Info routers
- Database is initialized on first request; applies migrations from `migrations/`

## Boundaries

| In Scope | Out of Scope |
|----------|--------------|
| REST API (FastAPI) | Authentication |
| React SPA | Deployment / CI |
| SQLite persistence | E2E testing |
| i18n (EN + EL) | WebSockets |

## API Surface

| Method | Path | Handler | Description |
|--------|------|---------|-------------|
| GET | `/health` | `health.py` | Returns `{"status": "ok"}` |
| GET | `/api/info` | `info.py` | Returns app name and version |

## Data Model

No feature tables yet. Only the migration placeholder exists. Schema grows as tasks add features.
