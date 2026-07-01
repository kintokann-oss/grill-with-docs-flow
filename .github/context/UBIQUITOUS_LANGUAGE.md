# Ubiquitous Language

> **What is this file?**
> A glossary of domain terms that ALL agents, code, and humans must use consistently.
> If this glossary says "Pitch" means X, then every variable name, function name, route name,
> and conversation must use that exact word with that exact meaning.
>
> This prevents AI agents from using synonyms that drift from what the code actually calls things.
> It also prevents the same concept from having multiple names across the codebase.
>
> **Who updates it?**
> - **Planner** (Phase 1): adds new terms or refines existing ones during grilling sessions.
>   Every time a new domain concept emerges or an existing term's meaning shifts, it goes here.
>
> **Rules:**
> - Only add terms where confusion is possible (don't add obvious things)
> - Terms must match what's used in code (variable names, table names, route names)
> - If code uses a different name than the glossary, one of them must change
> - Definitions should be ONE sentence — if it needs a paragraph, it's probably two concepts

---

### App

**Definition:** The FastAPI backend application instance, configured in `main.py`.

**Not:** The frontend React application (that's WebApp).

---

### WebApp

**Definition:** The React single-page application served by Vite in development.

**Not:** The backend API (that's App).

---

### Route

**Definition:** An HTTP endpoint handler registered on the FastAPI app via a router.

**Relationships:** Routes call Services. Routes live in `apps/api/src/routes/`.

---

### Service

**Definition:** A pure domain function that encapsulates business logic and database queries.

**Relationships:** Called by Routes. Lives in `apps/api/src/services/`. No HTTP types.

**Not:** An API client (that's a frontend concept).

---

### Migration

**Definition:** A SQL file in `apps/api/migrations/` that modifies the database schema. Applied in filename-sorted order.

---

### Component

**Definition:** A React function component with its own folder, styles, and colocated test.

**Relationships:** Lives in `apps/web-react/src/components/ComponentName/`.

---

### Hook

**Definition:** A custom React hook (`use*`) that encapsulates stateful logic or data fetching.

**Relationships:** Lives in `apps/web-react/src/hooks/`. May call API clients.

---

### API Client

**Definition:** A thin fetch wrapper in `apps/web-react/src/api/` that calls a backend Route.

**Not:** A Service (services are backend-only).

---

### Token (Theme Token)

**Definition:** A CSS custom property (`--token-name`) defined in `theme.css` that provides a design value (color, spacing, typography).

**Relationships:** Component CSS references tokens via `var(--token-name)`.

---

*Add new terms here as the Planner identifies them during grilling sessions.*
