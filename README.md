# Strapi 5 Implementation Skill

A Codex skill for implementing, debugging, and reviewing Strapi 5 host applications. It guides work on schemas, APIs, business logic, integrations, and admin UI while preserving document semantics, authorization, data integrity, and the native admin experience.

The authoritative agent instructions are in [SKILL.md](SKILL.md). This README explains when to use the skill and what to expect.

## When to use it

- Create or change application-owned content-types, components, APIs, controllers, and services.
- Diagnose document operations, relations, localization, publication behavior, or integration failures.
- Implement domain rules shared across API, Content Manager, imports, and background work.
- Build or improve application admin pages, settings, and editors.
- Review an implementation or prepare a plan without modifying the project.

Reusable plugin creation, packaging, custom-field plugins, and substantial `src/extensions` work primarily belong to the companion `strapi-v5-plugin-implementation` skill when available. This skill does not require that companion to be installed.

## Usage

Make this folder available in your Codex skills directory, then name the skill in your request or ask for work matching its scope. Provide the target project and the behavior you want to implement, investigate, or review.

Example requests:

```text
Use $strapi-v5-implementation to add an application endpoint.
Inspect the existing authorization and response contracts first.
```

```text
Use $strapi-v5-implementation to investigate why a localized document
update triggers the external integration twice. Reproduce and fix it.
```

```text
Use $strapi-v5-implementation to plan a new content-type and admin
editor. Do not implement anything yet.
```

This folder contains guidance and references, not a runnable Strapi application. Builds, tests, and runtime verification use the target project's tools and installed dependencies.

## Workflow

1. **Inspect the actual system.** Read project instructions and existing changes; identify installed versions and trace the relevant schema, request, service, document, and UI paths.
2. **Establish ownership and contracts.** Identify acceptance criteria, callers, permissions, domain invariants, compatibility requirements, and existing mechanisms to reuse.
3. **Implement within the requested scope.** Keep business rules in application-owned layers and verify version-sensitive APIs against primary documentation and installed source/types.
4. **Verify behavior.** Run focused checks, inspect effective schemas and persisted Content Manager configuration, and verify rendered admin behavior for UI changes.
5. **Report evidence.** Distinguish source checks, tests, runtime verification, deployment, and anything still unverified.

## Core conventions

- Prefer Document Service for content document operations; handle `documentId`, locale, publication state, relations, and side effects explicitly.
- Enforce authorization and ownership on the server, with input validation and output sanitization for the actual model and caller.
- Include meaningful `config.metadatas` for declared attributes in every schema JSON created or changed, and verify the resulting editor.
- Evaluate content-types first for independently managed persistent records; use components or JSON when their ownership and lifecycle fit.
- Prefer installed Strapi Design System controls and native layouts, including search, pagination, empty states, and loading feedback.
- Preserve existing versions, consumer contracts, unrelated changes, and the user's requested mode.

The detailed admin conventions are project preferences recorded by this skill; they are not all framework requirements. Component APIs must be checked against the installed version.

## References

| Reference | Covers |
| --- | --- |
| [Backend and data](references/backend-and-data.md) | Request ownership, document semantics, authorization, domain rules, migrations, and background work. |
| [Schemas and metadata](references/schemas-and-metadata.md) | Storage choices, field metadata, Content Manager configuration, and plugin schema extensions. |
| [Admin design](references/admin-design.md) | Native page layouts, fields, search, pagination, responsive behavior, and accessible states. |
| [Validation and sources](references/validation-and-sources.md) | Acceptance checks, behavioral skill evaluation, and source provenance. |

Read only the references relevant to the task.

## Scope and verification

A plan or review does not authorize implementation. An implementation request alone does not authorize production migrations, permission grants, commits, publication, or deployment.

Verification should match the change: API work checks allowed and denied callers and data boundaries; schema work checks compatibility and effective configuration; UI work checks the actual rendered route and relevant interaction states. Compilation alone is not runtime or admin UI proof.
