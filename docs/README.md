# HarvestLink documentation

This directory is the maintained implementation reference for HarvestLink. Update the relevant document whenever a feature changes, especially when a change affects stored data, role permissions, routes, or shared UI behavior.

## Documents

- [Data model](DATA_MODEL.md) — entities, relationships, constraints, indexes, lifecycle rules, and the path from the prototype to a persistent database.
- [Project context](PROJECT_CONTEXT.md) — product purpose, supported workflows, architecture, routes, decisions, limitations, and delivery checklist.
- [Style guide](STYLE_GUIDE.md) — visual tokens, typography, layout, components, interaction states, responsive rules, accessibility, and writing style.
- [Common patterns](COMMON_PATTERNS.md) — conventions for data access, authorization, routing, rendering, modals, validation, ratings, testing, and deployment.

## Maintenance rules

1. Treat these documents as part of the feature, not as retrospective notes.
2. Add or change a data field in [Data model](DATA_MODEL.md) before implementing a database migration.
3. Record durable product or architecture decisions in [Project context](PROJECT_CONTEXT.md).
4. Reuse the tokens and component rules in [Style guide](STYLE_GUIDE.md) instead of introducing one-off CSS.
5. Follow [Common patterns](COMMON_PATTERNS.md) when adding interactions so role checks and state transitions remain consistent.
6. Keep examples free of real credentials, personal data, and production secrets.

## Current source of truth

The deployed prototype is a static browser application. Its state is seeded in `app.js` and persisted under the local-storage key `harvestlink-prototype-v1`. The persistent database design in these documents is the intended future model; it is not yet connected to the deployed application.

