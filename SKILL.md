---
name: strapi-v5-implementation
description: Implement, debug, or review Strapi 5 application schemas, APIs, business logic, integrations, and admin customization. Use for host-project development. Reusable plugin creation, packaging, custom-field plugins, and src/extensions work primarily belong to strapi-v5-plugin-implementation.
---

# Strapi 5 application implementation

Build application-owned behavior that preserves Strapi's document semantics, authorization, data integrity, and native admin experience. Follow the requested mode: a review or plan does not authorize edits or release actions.

## Start with the actual system

- Read applicable project instructions and changes already present. Identify installed Strapi, Node, database, plugin and admin dependencies; do not silently upgrade them.
- Trace the relevant schema → route/policy → controller → service → document operation → integration/UI path. Confirm which layer owns each rule and which consumers depend on it.
- Establish the intended user journey, acceptance criteria, permission boundary, and evidence needed. Inspect existing mechanisms before introducing new ones.
- Use current primary docs plus installed source/types for version-sensitive behavior. Current documentation does not establish availability in every Strapi 5 minor.

## Required conventions

- Keep host business rules in application-owned services/integrations. Keep reusable plugin code neutral; put host-specific plugin schema changes in persisted extension JSON. If available, use `strapi-v5-plugin-implementation` for substantial plugin/extension work; this skill does not depend on its installation.
- Prefer Document Service for document operations. Handle `documentId`, locale, publication state, relation semantics, and middleware side effects explicitly. Database queries and lifecycle hooks are not interchangeable with document operations.
- Validate inputs and sanitize outputs for the correct content-type and caller. Domain ownership checks belong on the server; UI restrictions and direct Document Service calls do not supply authorization automatically.
- Every schema JSON created or changed must include meaningful `config.metadatas` for its declared attributes. Verify persisted Content Manager configuration and the rendered editor, not just JSON or generated types.
- For addable persistent data, evaluate content-types first; use components/JSON when ownership and lifecycle justify it. Admin visibility, private attributes, route exposure, and permissions are separate decisions.
- Admin UI uses Strapi Design System and native page layouts. Preserve responsive gutters/header actions, clear field alignment, and accessible states. Repeatable item editors default to Accordion; buttons beside normal inputs use `size="L"`. These are user-selected implementation conventions, not claims that Strapi enforces them.

- Listing pages use a localized `Layouts.Header subtitle` such as "2 entries found" when the result total is positive (singular for one); preserve the existing descriptive subtitle for zero results. Use the filtered total, not only the current page length. Inner detail pages put useful document/session IDs in the header subtitle instead of a separate body row. Header `primaryAction` buttons keep the native default size; do not set `size="M"`.
- Search uses native `Searchbar` with the built-in admin interaction: at medium (768px) and above, start with a fixed-width search icon button matching the searchbar/companion-field height; companion fields stretch into available space. Click opens and focuses the searchbar. Blur collapses only an empty query; preserve nonempty queries. Below medium, always show a full-width searchbar without the icon toggle. Preserve Enter, clear/Escape, accessible labels, and pagination reset. See the admin-design reference.

## Component selection and loading

- Use Strapi Design System `EmptyStateLayout` with a localized `content` message for empty admin collections, archive lists, and table results. Keep empty states distinct from loading, errors, and permission denial; preserve meaningful messages for no matching results versus unavailable data. Use optional native `icon` or `action` only when the workflow calls for it.
- For section-level loading, prefer `EmptyStateLayout` with localized loading `content` and a native `Loader` in its supported `icon` prop (`icon={<Loader>Loading label</Loader>}`), not `startIcon`. Give the loading region `role="status"` and `aria-busy="true"`; show no-results content only after loading completes. Keep native full-page loading for page initialization and retain existing results during background refreshes when appropriate. Verify props against the installed Design System version.

- Give installed Strapi Design System components the highest priority for admin UI. Use the matching native control, including `Searchbar` for search and `TimePicker` for time selection. Fall back to a custom control only when the installed Design System has no suitable component or its capabilities do not match the actual requirement; document the mismatch and preserve native tokens and accessibility. Inspect supported props and match companion field heights at every responsive breakpoint.
- Always use Strapi Design System `Pagination` with its native `PreviousLink`, `PageLink`, `NextLink`, and `Dots` controls for admin pagination. Hide pagination when the filtered total fits in one page (`pageCount <= 1`, including exactly one full page); derive the threshold from the API page size, not a hard-coded item count. Preserve query filters in navigation links, support URL state and browser Back/Forward, and respect cursor APIs without inventing total page counts. Verify boundary states, keyboard access, and responsive layout against installed types.
- Standard `Button` uses its native `loading` prop. Do not put a custom `Loader` in its children or `startIcon`; retain the action label and normal icon.
- `IconButton` has no `loading` prop in the current Design System. A small 24px by 24px `Loader` is allowed, but preserve the original outer dimensions by reducing padding: S/M 32px square with 4px padding; L 40px square with 8px padding. Ordinary icons are 16px square. Check border-box sizing and rendered dimensions against the installed version; loading must not enlarge the button.

## Read only the relevant references

| Work | Reference |
|---|---|
| APIs, document semantics, domain rules, migrations, background work | [Backend and data](references/backend-and-data.md) |
| Content-types, components, field metadata, plugin schema extensions | [Schemas and metadata](references/schemas-and-metadata.md) |
| Any admin page, settings page, or field editor | [Admin design](references/admin-design.md) |
| Verification, realistic skill evaluation, source provenance | [Validation and sources](references/validation-and-sources.md) |

## Finish with evidence

Run focused checks appropriate to the change; verify rendered admin behavior for UI changes. Distinguish source checks, tests, runtime verification, and deployment. Report unverified behavior plainly. Do not change unrelated plugins, grant permissions, run production migrations, commit, publish, or deploy merely because implementation was requested.
