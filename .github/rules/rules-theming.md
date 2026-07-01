# Theming Rules

**Scope:** Frontend CSS · **Token file:** `apps/web-react/src/styles/theme.css`

## Core Rule

Only `theme.css` may contain raw color values (`#`, `rgb()`, `rgba()`, `hsl()`, `hsla()`).

Everywhere else: use `var(--token-name)` only.

## Token Layout

Organize `:root` in this order:

1. **Raw palette** — `--palette-neutral-*`, `--palette-primary-*`
2. **Semantic colors** — `--color-bg`, `--color-text`, `--color-error`, surface tokens
3. **Spacing** — `--space-1` through `--space-8`
4. **Typography** — `--font-family-sans`, `--font-size-*`, `--line-height-*`
5. **Radius, shadow, z-index** — `--radius-*`, `--shadow-*`, `--z-*`

## Adding a New Color

1. Add palette step in `theme.css` if needed
2. Add semantic token referencing palette
3. Use semantic token in component CSS — never add raw color values in component files

## Component CSS

- Use semantic tokens exclusively
- Component-level CSS is colocated in the component folder
- No inline styles that should be tokens
