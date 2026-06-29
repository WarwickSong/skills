---
name: "vue-vite-product-ui-review"
description: "Reviews and improves Vue 3 + Vite product UI. Invoke for frontend pages, forms, navigation, upload, feed, dashboard, and UX changes."
---

# Vue Vite Product UI Review

This skill guides Vue 3 + Vite frontend product work. It is based on `zhihua-dev-profile`, especially practical product UI, role-aware workflows, media/upload flows, and API-backed states.

Use it when changing Vue pages, components, routing, API calls, forms, upload flows, feed/detail views, dashboards, profile/company-like pages, role-based flows, or user-facing states.

## Inferred Frontend Tendencies

- Product clarity matters more than decorative complexity.
- State clarity is essential: loading, empty, success, error, unauthorized, disabled, and completed states should be explicit.
- Role-based or workflow-heavy pages should make the user's current path obvious.
- UI should remain simple and maintainable unless the user asks for a broader redesign.
- Frontend should respect backend contracts and handle API errors visibly.

## Generic Context Checks

Before meaningful frontend changes:

1. Read repository instructions and overview docs.
2. Inspect `package.json` and current dependencies.
3. Identify routing, API client, component, composable/store, style, and page conventions.
4. Read product, frontend, API contract, or acceptance docs if present.
5. Reuse existing components and styles before adding new abstractions or dependencies.
6. Use `mature-package-first-dev` before adding frontend libraries.
7. Use `code-runtime-env` before running builds, tests, package managers, or installing dependencies.

## UI Review Dimensions

Evaluate UI changes across these dimensions:

- Product clarity: user can tell what object or workflow they are viewing or editing
- Role clarity: different user roles or permissions are not confused
- State clarity: loading, empty, success, error, unauthorized, disabled, and completed states are visible
- Navigation clarity: users can return to core flows without dead ends
- Form quality: labels, validation, disabled states, submit feedback, and recovery are clear
- Mobile usability: layouts work for narrow screens when relevant
- Accessibility basics: semantic buttons/links, keyboard reachability, visible focus, useful alt text when relevant
- API robustness: frontend handles backend error responses without silent failure
- Consistency: styles and structure match existing views/components

## Mature Package Rule

Before adding a frontend dependency:

1. Check whether Vue/Vite/browser APIs already solve it.
2. Check whether existing components, CSS, composables, or API utilities can be extended.
3. Only add a package if it clearly improves UX or reduces substantial complexity.
4. Consider bundle size, maintenance, license, and whether the dependency is overkill.

## Implementation Guidance

- Keep components focused and readable.
- Prefer explicit state names over clever abstractions.
- Keep API calls in the project's existing API/composable layer when possible.
- Keep route behavior aligned with the existing router.
- Preserve existing simple styling unless the user asks for a broader visual redesign.
- For major UI changes, run or recommend the project build command through `code-runtime-env`.
- Use `project-doc-sync` when user-facing behavior, API expectations, or acceptance criteria change.

## Output Expectations

When reviewing or changing UI, report:

- Affected user flow
- Main UX risk addressed
- Existing component/pattern reused
- Validation path such as build or manual flow check
- Docs updated if product behavior changed
