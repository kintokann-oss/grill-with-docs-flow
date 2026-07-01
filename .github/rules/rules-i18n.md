# i18n Rules

**Scope:** Frontend copy · **Stack:** react-i18next · **Locales:** `apps/web-react/src/i18n/locales/`

## Core Rule

No user-facing string literals in UI source. Use `t('key')` for everything:
- Labels, titles, button text
- `aria-label`, `placeholder`
- Error messages, descriptions

## Setup

i18n init runs in the app entry before render.

## Usage

```tsx
const { t } = useTranslation('app');
<button>{t('actions.save')}</button>
```

## Key Shape

`namespace.section.key` — e.g. `app.actions.save`

## Adding New Copy

1. Add key to all locale JSON files (`en.json`, `el.json`, etc.)
2. Use the key in component via `t('key')`
3. Stub translations for non-primary locales if unsure of correct translation

## Tests

- Use `i18n.t('namespace:key')` for expected text in assertions
- Do not hardcode locale strings in test expectations
