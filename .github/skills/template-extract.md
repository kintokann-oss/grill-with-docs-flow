# Skill: Template Extraction

## Purpose

Derive reusable document templates from example company documents (PDFs, Word docs, filled-out forms). Enables the Requirements Agent to standardize how requirements are captured for a specific organization.

## When to Use

- When a user provides an example requirements document from their company
- When onboarding a new project that follows a specific documentation format
- When standardizing ad-hoc documents into repeatable templates

## Method

### 1. Structural Analysis

Examine the source document and map its anatomy:

```yaml
document_structure:
  - level: 1
    title: "<section name>"
    content_type: header | narrative | table | list | form_fields
    repeats: true | false
    children:
      - level: 2
        ...
```

Identify:
- **Fixed elements:** Headers, boilerplate, legal text that never changes
- **Variable elements:** Content that differs per document instance
- **Conditional elements:** Sections that appear only in some cases
- **Calculated elements:** Content derived from other fields

### 2. Field Extraction

For each variable element, determine:
- **Field name:** A clear identifier (snake_case)
- **Type:** text, date, number, list, table, enum, reference
- **Required vs optional**
- **Constraints:** max length, allowed values, format patterns
- **Source:** Who/what provides this information

### 3. Template Generation

Create a markdown template where:
- Fixed content is written literally
- Variable content uses `{{field_name}}` placeholders
- Conditional sections use `{{#if condition}}...{{/if}}`
- Repeating sections use `{{#each collection}}...{{/each}}`

### 4. Schema Generation

Create a YAML schema that formally defines all fields:

```yaml
schema:
  name: "<template name>"
  version: 1
  fields:
    - id: field_name
      label: "Human-readable label"
      type: text | date | number | list | table | enum
      required: true | false
      constraints:
        max_length: 500
        pattern: "regex if applicable"
      description: "What this field captures"
```

### 5. Validation

Regenerate the original document using the template + extracted field values.
Compare with the source. If anything is missing or distorted, refine the template.

## Output Files

| File | Purpose |
|------|---------|
| `.github/templates/<name>.template.md` | The reusable markdown template |
| `.github/templates/<name>.schema.yaml` | Structured field definitions |
| `.github/templates/<name>.example.md` | Filled example matching the original |

## Anti-Patterns

- **Don't over-abstract** — if a section only appears in one example, keep it concrete
- **Don't merge distinct templates** — if two documents serve different purposes, make two templates
- **Don't ignore formatting** — numbering schemes, header levels, and ordering matter for compliance
