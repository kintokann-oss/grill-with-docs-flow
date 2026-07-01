# Frontend Rules

**Scope:** `apps/web-react/` · **Stack:** React + TypeScript + Vite

## File Layout

| Path | Role |
|------|------|
| `src/components/` | UI components — one folder per component |
| `src/hooks/` | Custom hooks — `use*` naming |
| `src/api/` | Thin fetch wrappers for backend endpoints |
| `src/styles/` | Global CSS + `theme.css` (tokens only) |
| `src/i18n/` | react-i18next setup and locale JSON files |
| `src/App.tsx` | Root page shell |

## Components

- One folder per component: `ComponentName/ComponentName.tsx` + colocated `.css`
- Export a named function component
- Props type as `ComponentNameProps`
- Colocate test: `ComponentName.test.tsx`

## Hooks

- Name `use*`; place in `src/hooks/`
- Encapsulate data loading, API calls, and local UI state
- Colocate test: `useHookName.test.ts`

## API Clients

- Thin `fetch` wrappers in `src/api/` — one module per backend resource
- Paths must match backend route definitions
- Colocate test: `resource.test.ts`

## State Management

- Prefer custom hooks over global stores unless plan specifies otherwise
- Server state flows: hook → API client → backend

## Styling

- `var(--token-name)` only outside `theme.css`
- Component CSS uses semantic tokens, never raw color values (`#`, `rgb()`, `hsl()`)

## Copy (i18n)

- No user-facing string literals in JSX/TSX
- Use `useTranslation('namespace')` + `t('key')`
- New copy: add keys to locale files

## Testing

- Write tests alongside implementation (colocated)
- Use Vitest + React Testing Library
- AAA pattern: Arrange → Act → Assert
- Use `i18n.t('key')` for expected text, not hardcoded strings
- Tests must pass before reporting step complete
