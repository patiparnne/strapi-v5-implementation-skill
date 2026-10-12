# Backend and data

## Ownership and request flow

Keep transport parsing, response shaping, and caller context in controllers; reusable domain rules in services; credentials and remote adapters in infrastructure modules. Reuse Strapi factories where they preserve the desired contract. When extending a core action, retain its validation/sanitization and response behavior or explicitly replace those protections.

Before changing a write path, identify all callers: REST, Content Manager, GraphQL if installed, internal services, imports, and workers. A rule implemented only in a custom REST controller does not protect writes through other paths. Place invariants at the narrowest shared layer that actually observes the required operations.

## Document semantics

- Prefer `strapi.documents(uid)` for content document CRUD. Use `documentId` at document/API boundaries; do not treat a database row ID as an interchangeable identifier.
- Select locale and draft/published behavior intentionally. One logical document may correspond to multiple rows; uniqueness, updates, and side effects must respect that distinction.
- Inspect the version's relation write contract before using connect/disconnect/set. Preserve relation direction, ownership, deletion behavior, and optional plugin requirements.
- Distinguish request middleware, Document Service middleware, and database lifecycle hooks. Verify invocation counts and which calls bypass each mechanism; prevent recursion and duplicate external effects.
- Use Query Engine/SQL only for a concrete need. Explain which validation, authorization, document semantics, and hooks it bypasses; provide equivalent necessary protections. Never use a low-level write merely to evade a valid error.

## Authorization and API shape

Admin sessions/RBAC, API tokens, and Users & Permissions users are different principals. Check route registration, auth strategy, scopes, policies, tenant/owner constraints, and output fields for the actual caller.

Custom queries and mapped content-types require validation/sanitization against the actual model, not the controller's original model. Do not pass arbitrary client filters, populate trees, field names, or writable attributes directly into database operations. Bound pagination and population. Prevent users from assigning ownership, publication, or other protected fields without authorization.

Keep errors actionable without leaking credentials or internal details. Preserve the application's established response contract; do not impose a new REST envelope on existing consumers. HTTP authentication is not a substitute for record-level authorization.

## Data changes and integrations

For schema changes, inspect existing records before adding required/unique constraints, renaming fields, or changing relation/cardinality/storage types. Plan a staged backfill, verification, and recovery where needed. Regenerate typings from the schema; do not hand-edit generated declarations to make nonexistent fields compile.

For retryable external effects, define idempotency keys and enforce relevant uniqueness atomically. Specify partial-failure behavior, timeouts, bounded retries, and observability. A database transaction does not roll back a message already sent to another system. Verify transaction support on the installed version instead of assuming every Strapi 5 release has the same API.

Workers must have deliberate startup/shutdown behavior, bounded work, safe restart handling, and multi-process coordination where relevant. Avoid unconditional sends, migrations, or destructive repair at application bootstrap. Exercise integrations against isolated fixtures or dry-run adapters; local application boot can have real side effects.

## Sources

- [Document Service](https://docs.strapi.io/cms/api/document-service)
- [Controllers and sanitization](https://docs.strapi.io/cms/backend-customization/controllers)
- [Middleware kinds](https://docs.strapi.io/cms/backend-customization/middlewares)
- [Error handling](https://docs.strapi.io/cms/error-handling)

Research baseline: 2026-09-29. Verify method signatures and supported features against the installed version before implementation.
