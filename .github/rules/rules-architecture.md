# Architecture Rules

## Monorepo Layout

```
apps/
├── api/                    # FastAPI backend (Python)
│   ├── migrations/         # SQL migration files
│   ├── src/
│   │   ├── main.py         # App entry, mount routers
│   │   ├── db.py           # Connection + init_db
│   │   ├── routes/         # HTTP handlers (thin)
│   │   └── services/       # Domain logic + queries
│   └── tests/              # pytest tests
│
└── web-react/              # React + TypeScript + Vite
    └── src/
        ├── App.tsx          # Root shell
        ├── api/             # Fetch wrappers per resource
        ├── components/      # UI components
        ├── hooks/           # Custom hooks
        ├── styles/          # Theme + global CSS
        └── i18n/            # Locales + setup
```

## Boundaries

| Layer | Directory | Responsibility |
|-------|-----------|----------------|
| Client | `apps/web-react/` | Pages, components, hooks, API clients, i18n, theme |
| Server | `apps/api/` | HTTP routes, domain services |
| Schema | `apps/api/migrations/` | DDL, seeds |
| Store | SQLite via `DATABASE_URL` | Tables created by migrations |

Client calls Server over HTTP JSON. Server accesses Store through services only.

## Key Rules

- Routes call services; services run SQL. No SQL in route handlers; no HTTP types in services.
- One router file per feature domain.
- One component folder per component.
- API clients are thin fetch wrappers — no business logic.
