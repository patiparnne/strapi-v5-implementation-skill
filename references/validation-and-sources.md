# Validation and source provenance

## Change acceptance

- API/domain change: allowed and denied principals, invalid input, ownership boundaries, private output, relevant document/publication/locale behavior, and important failure/retry cases.
- Schema change: parse/type checks, effective loaded schema, existing-data compatibility, generated types, new/existing Content Manager configuration, and rendered fields.
- UI change: exact route, header/body alignment, narrow/wide layouts, light/dark, permission/loading/error/empty states, keyboard interaction, and successful/failed saves.
- Background work: isolated adapters, bounded batches, concurrency/restart/duplicate delivery handling, and shutdown where relevant.

Start narrow, then broaden only for dependency impact or unresolved uncertainty. Do not boot a real application against production data to test a skill example. Report commands and outcomes without equating compilation with runtime/UI proof.

## Behavioral skill evaluations

When maintaining this skill, exercise realistic requests in isolated fixtures. Evaluate produced decisions/artifacts, not word matching. Do not give an independent evaluator these expected outcomes in its initial task.

| Request/fixture | Expected behavior |
|---|---|
| Add an application endpoint that reads a second model with a private attribute | Uses that model's authorization and sanitization, bounds queries, tests forbidden fields and denied principals. |
| Add a required field to a populated localized content-type | Assesses existing data and variants, plans/applies an authorized staged change, adds JSON metadata, regenerates types. |
| Add a service operation called from API and Content Manager | Places invariants where both paths enforce them; checks hook semantics and duplicate effects. |
| Change a label already customized in Configure View | Inspects saved configuration and applies only the intended metadata change; verifies the rendered editor. |
| Create a settings editor with addable rows and a save failure | Native layout/Accordion, stable row identities and accessible actions, draft preserved on failure. |
| Request only a plan or review | Does not implement, migrate, publish, or deploy. |
| Build a distributable plugin | Selects the plugin skill when available; does not put host-specific behavior into its reusable core. |

## Provenance and limits

Research on 2026-09-29 inspected a Strapi 5.42.1 application and its local plugins. These source examples informed patterns; they are not installation dependencies or complete best-practice templates:

- Loyalty `admin/src/pages/HomePage.tsx`: native header/content structure and action placement.
- SMS `admin/src/pages/SessionPage.tsx`: Back navigation and save action slots.
- SMS `admin/src/pages/Settings/Integration.tsx`: content-type/field selection instead of implicit host binding.
- Loyalty reward `schema.json`: explicit edit/list metadata.
- LINE Messaging: separation of content-types, server services, admin workspace, and a custom field. Route access must be traced through final registration; filenames alone do not establish exposure.

Do not copy unrelated business fields, unbounded queries, raw controls, or package ranges from these examples. No access to the originating repository is required to use this skill. The reference pages in this skill carry official links and version-sensitive caveats.
