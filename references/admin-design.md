# Native Strapi admin design

These are required user conventions. They supplement the versioned Strapi APIs rather than claiming the framework mandates every layout choice.

## Page recipes

Use `Page` and `Layouts` from the target version's supported Strapi admin exports and controls from `@strapi/design-system`; use `@strapi/icons`. Inspect installed types and built-in pages before using a new prop. Avoid deprecated v4 helper-plugin APIs and private deep imports.

| Surface | Composition |
|---|---|
| Home/list workspace | Native page title and header; primary actions in the header's right action slot; body directly in native content gutters; useful toolbars/tables/sections without decorative card-inside-card nesting. |
| Inner/create/edit page | Native header `navigationAction` for Back; `primaryAction` for Save/Create/Submit; title and body align with the host admin. Use a predictable parent destination and handle unsaved changes. |
| Settings page | Same native responsive header/content spacing. Grouped panels are appropriate for related configuration; retain a clear save model and hierarchy. |

The inspected newer Strapi layout uses shared responsive spacing between header and content. Prefer native layouts over hard-coded padding or extra outer wrappers. Older supported minors may have different layout capabilities; match the host's built-in layout and verify before claiming responsive equivalence.

## Header subtitles and actions

When the page's main purpose is listing entries, show a localized result-total subtitle such as "2 entries found" for positive totals, with correct singular/plural forms. Use the filtered total across pages; preserve the existing subtitle for no entries. On inner detail pages, put useful document/session IDs in `Layouts.Header subtitle`, not a separate body row. Header `primaryAction` buttons use their native default size; omit `size="M"`.

## Fields and layout

- Use Strapi Design System `EmptyStateLayout` with a localized `content` message for empty admin collections, archive lists, and table results. Keep empty states distinct from loading, errors, and permission denial; preserve meaningful messages for no matching results versus unavailable data. Use optional native `icon` or `action` only when the workflow calls for it.
- For section-level loading, prefer `EmptyStateLayout` with localized loading `content` and a native `Loader` in its supported `icon` prop (`icon={<Loader>Loading label</Loader>}`), not `startIcon`. Give the loading region `role="status"` and `aria-busy="true"`; show no-results content only after loading completes. Keep native full-page loading for page initialization and retain existing results during background refreshes when appropriate. Verify props against the installed Design System version.

- Give installed Strapi Design System components the highest priority. Use native `Grid`, `Flex`, `Box`, `Field`, inputs, typography, and theme tokens; use `TimePicker` for time selection. Fall back to a custom control only when no suitable native component exists or its supported behavior does not fit the requirement. Explain that mismatch, preserve native tokens and accessibility, and verify the custom fallback in the actual admin. Minimal custom layout styling must preserve native tokens and accessible semantics.
- Align field baselines, labels, hints, and validation rows intentionally. Use full-width fields in grid cells and responsive column collapse; avoid fixed widths that create page overflow.
- Buttons beside normal-height fields use `size="L"`, with bottom/baseline alignment as appropriate. Verify actual control height in the installed version. A compact add button beside a group label is a different context.
- Prefer one clear primary action, concise labels, progressive disclosure, and meaningful defaults. Display schema identifiers only when useful to configuration decisions.
- Include loading, empty, recoverable error, saving, success, and no-permission states. Preserve drafts on failed saves, prevent duplicate submissions, and expose field-specific validation.
- Namespace translations, support long/localized labels, and use theme tokens that work in light/dark mode. Provide accessible names for icon-only actions and connect labels/hints/errors to inputs.

## Repeatable editor recipe

1. Place the field-group label left and a plus-icon **Add** button right (with an accessible label identifying the item type).
2. Render each item in Design System Accordion. Use a useful item summary as the trigger label. Maintain stable identity across edit/remove/reorder, not array indices as identity.
3. Place a labelled trash `IconButton` in the item header beside, not inside, the accordion trigger. Use `type="button"`; clicking it must not submit the form or toggle the accordion accidentally.
4. Only when reorder is required, put a six-dot drag handle before the item label. Persist order, provide keyboard operation or accessible move controls, and preserve focus and expanded state across movement.
5. Expand and focus a newly added item; expose errors inside collapsed items, opening/focusing the first invalid field on submit when appropriate. Restore focus predictably after removal. Handle empty/read-only/disabled states.

For destructive deletion of saved records, follow the feature's confirmation/recovery model. Removing an unsaved row need not force a confirmation dialog. Do not impose single-open or multiple-open behavior without considering the editing task.

## Verification

Compare the rendered page with a built-in page in the same installed admin, including header gutters and action placement. Test wide and narrow viewports, light/dark themes, long labels, validation text, keyboard navigation, and real route transitions. A build or bounding-box assertion does not prove professional UI. Record limitations if a required browser/runtime is unavailable.

## Sources

- [Design System](https://design-system.strapi.io/?path=/docs/getting-started-welcome--docs)
- [Accordion actions and accessibility](https://design-system.strapi.io/?path=/docs/components-accordion--docs)
- [Responsive values](https://design-system.strapi.io/?path=/docs/foundations-responsive--docs)
- [Admin Panel API](https://docs.strapi.io/cms/plugins-development/admin-panel-api)

Research baseline: 2026-09-29. Component composition and responsive props must be checked against the actual dependency version.

## Search and asynchronous button states

Use native `Searchbar` with the built-in admin search interaction. At medium (768px) and above, an empty, unfocused search initially renders as a fixed-width search icon button matching the searchbar and companion-field height. Let companion fields stretch to consume the remaining toolbar width. Clicking the icon reveals and focuses the searchbar. Collapse on blur only when the query is empty; preserve a nonempty searchbar. Below medium, always render a full-width searchbar and hide the icon toggle. Use the installed theme's medium breakpoint and verify the 767px/768px boundary. Preserve accessible labels, Enter submission, clear/Escape behavior, and pagination reset when applying or clearing a query. Match outer heights to adjacent fields; do not assume Searchbar defaults match normal inputs. In the His & Her messaging workspaces, the target is 50px below md and 40px at md (768px) and above; other hosts follow their own companion controls.

For standard `Button`, use `loading={pending}` with its normal action label and icon. Never substitute a custom Loader for Button children or startIcon. For `IconButton`, which lacks that prop in the inspected version, keep an accessible busy label and disabled state as appropriate. If inserting a 24px small Loader instead of the normal 16px icon, explicitly preserve the outer size (S/M 32px, L 40px) and reduce padding (4px or 8px respectively, adjusted for borders as necessary). Verify no layout shift while toggling loading, including responsive sizing.

## Pagination

Always use installed Strapi Design System `Pagination` and its native `PreviousLink`, `PageLink`, `NextLink`, and `Dots`. Hide pagination when the filtered total fits in one page (`pageCount <= 1`, including exactly one full page); derive the threshold from the API page size, not a hard-coded item count. Use actual links integrated with the host router; preserve filters and unrelated URL parameters, restore state on reload and browser Back/Forward, and reset the listing position when filters change. For cursor APIs, show only available navigation and never fabricate a total page count. Verify disabled boundaries, accessible labels, narrow layouts, and light/dark themes.
