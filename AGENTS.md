# HarvestLink agent instructions

## Scope

These instructions apply to the entire repository. More specific `AGENTS.md` files may be added in subdirectories later; when present, the nearest applicable file takes precedence.

## Start here

Read [the documentation index](docs/README.md), then open only the documents relevant to the task:

- Data entities, relationships, constraints, authorization, or persistence: [Data model](docs/DATA_MODEL.md)
- Product purpose, supported behavior, architecture, routes, limitations, or roadmap: [Project context](docs/PROJECT_CONTEXT.md)
- Visual design, components, responsive behavior, accessibility, or interface copy: [Style guide](docs/STYLE_GUIDE.md)
- Implementation, state, validation, modal, rating, testing, or deployment conventions: [Common patterns](docs/COMMON_PATTERNS.md)

Treat those documents as the canonical source. Do not reproduce their content in this file, code comments, or additional documentation. Link to and update the canonical section instead.

## Working contract

- Preserve existing user work and unrelated changes. Inspect repository status before editing and keep changes scoped to the request.
- Follow the documented prototype architecture unless the task explicitly authorizes an architectural migration.
- Keep existing browser data compatible when changing the prototype model. For a production database change, update the canonical data model and use a versioned migration.
- Do not introduce a new token, component convention, entity rule, route, or shared implementation pattern without updating its owning document.
- Never commit credentials, personal data, access tokens, generated dependency folders, or machine-specific configuration.
- Use the repository's established editing and verification tools. Fix failures caused by the change; report unrelated failures clearly.
- Do not claim completion until the applicable quality, role-flow, and deployment checks referenced by the documentation have passed.

## Documentation ownership

Route each durable change to one canonical document:

| Change | Canonical document |
| --- | --- |
| Entity, field, relationship, lifecycle, constraint, index, or authorization rule | [Data model](docs/DATA_MODEL.md) |
| Feature status, role capability, architecture, route, decision, limitation, or roadmap | [Project context](docs/PROJECT_CONTEXT.md) |
| Token, layout, component, UI state, responsive rule, accessibility rule, or content convention | [Style guide](docs/STYLE_GUIDE.md) |
| Reusable code, state, validation, interaction, testing, or deployment approach | [Common patterns](docs/COMMON_PATTERNS.md) |
| Document discovery or maintenance policy | [Documentation index](docs/README.md) |

When a change spans categories, update each affected canonical document with links between them where useful. Avoid copying the same explanation into multiple files.

## Completion handoff

In the final handoff, summarize the outcome, verification performed, and any remaining limitation or migration consequence. Link directly to changed repository files. Keep implementation detail in the canonical documentation rather than expanding this file.

